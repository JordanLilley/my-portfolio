<template>
  <div class="bg-black h-screen px-6 py-12">
    <div>
      <h2
        class="text-white font-exo text-5xl pb-4 opacity-0"
        ref="title"
        style="transform: translateY(30px)"
      >
        PREVIOUS WORK
      </h2>
      <div class="flex space-x-4">
        <div
          ref="projects"
          v-for="project in projects"
          class="w-full opacity-0"
          style="transform: translateX(30px)"
        >
          <a :href="project.url">
            <img
              :src="project.image.url"
              alt=""
              style="height: 70vh"
              class="object-cover"
            />
            <p class="font-space-mono text-white text-sm">
              {{ project.title }}
            </p>
          </a>
        </div>
      </div>
    </div>
    <div style="display: flex; justify-content: center">
      <div class="center">
        <a
          id="homeButton"
          href="/"
          class="border px-4 py-2 rounded-lg text-sm text-white font-space-mono uppercase mt-14 hover:bg-white hover:text-gray-800 inline-block opacity-0"
          style="transform: translateY(30px)"
        >
          GO BACK
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import gsap from "gsap";
// import ethereumImg from "~/assets/ethereum.jpg";
import ethereumImg from "~/assets/ethereumCarbonCalculatorPage.png";
import missingImage from "~/assets/questionMark.jpg";
export default {
  data() {
    return {
      projects: [
        {
          image: {
            url: ethereumImg,
          },
          title: "ETHEREUM CARBON CALCULATOR",
          url: "https://ethereum-carbon-calculator-kdrnj.ondigitalocean.app/",
        },
        {
          image: {
            url: missingImage,
          },
          title: "MORE TO COME",
          url: "/",
        },
      ],
    };
  },
  mounted() {
    gsap.to(this.$refs.title, {
      opacity: 1,
      duration: 2,
      y: 0,
      ease: "expo",
    });

    gsap.to(this.$refs.projects, {
      opacity: 1,
      duration: 2,
      stagger: 0.1,
      x: 0,
      ease: "expo",
    });

    gsap.to("#homeButton", {
      opacity: 1,
      duration: 1.5,
      delay: 0.6,
      y: 0,
      ease: "expo",
    });
  },
};
</script>
