<template>
  <div class="main-container">
    <div class="canvas-container">
      <TresCanvas clear-color="#87CEEB" shadows>
        <TresPerspectiveCamera :position="[5, 5, 10]" :look-at="[0, 0, 0]" />

        <TresDirectionalLight
          :position="[5, 10, 5]"
          :intensity="2"
          cast-shadow
        />
        <TresAmbientLight :intensity="0.5" />

        <Suspense>
          <Environment preset="sunset" />
          <primitive object="{model}" ref="houseRef" />
        </Suspense>

        <ContactShadows :opacity="0.4" :blur="2.5" />
        <OrbitControls enable-zoom="false" />
      </TresCanvas>
    </div>

    <div class="scroll-container">
      <section class="h-screen">NEW SECTION</section>
    </div>
  </div>
</template>

<script async setup>
import { shallowRef, onMounted, Suspense } from "vue";
import { TresCanvas } from "@tresjs/core";
import {
  ContactShadows,
  Environment,
  OrbitControls,
  useGLTF,
} from "@tresjs/cientos";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

let houseRef = shallowRef(null);

let { scene: model } = await useGLTF("@/assets/GLB/autumn_house.glb");

onMounted(() => {
  if (houseRef.value) {
    let timeline = gsap.timeline({
      scrollTrigger: {
        trigger: ".scroll-container",
        start: "top top",
        end: "bottom bottom",
        scrub: 1,
      },
    });

    timeline.to(houseRef.value.rotation, { y: Math.PI * 2, duration: 10 });
  }
});
</script>

<style lang="scss" scoped>
.canvas-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  z-index: -1;
}
section {
  height: 100vh; /* Full screen scrolling */
}
</style> 