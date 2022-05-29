<template>
  <body>
    <div class="xl:flex h-screen">
      <div
        id="infoDiv"
        class="xl:w-1/2 flex flex-col justify-center px-8 bg-black pt-16 xl:pt-0"
      >
        <div
          id="infoDivData"
          style="transform: translateX(-100px); max-width: 500px"
          class="mx-auto"
        >
          <h1 class="text-white l:text-3xl mb-4 font-exo">CONTACT EMAIL:</h1>
          <div class="text-gray-400 mb-8 font-play l:text-2xl">
            <p>jordan.lilley@icloud.com</p>
          </div>

          <h1 class="text-white l:text-3xl mb-4 font-exo">LINKEDIN:</h1>
          <div class="text-gray-400 mb-8 font-play l:text-2xl">
            <a href="https://www.linkedin.com/in/jordan-l-42ba3815b/"
              >https://www.linkedin.com/in/jordan-l-42ba3815b/</a
            >
          </div>

          <div>
            <a
              href="/"
              class="text-white border px-4 py-2 rounded-lg text-sm font-space-mono uppercase mt-8 hover:bg-white hover:text-gray-800 inline-block"
              id="homeButton"
            >
              <b>GO HOME</b>
            </a>
          </div>
        </div>
      </div>
      <div id="globeDiv" class="h-screen xl:w-1/2">
        <canvas></canvas>
      </div>
    </div>
  </body>
</template>

<script>
import gsap from "gsap";
import {
  Scene,
  PerspectiveCamera,
  WebGLRenderer,
  Mesh,
  SphereGeometry,
  MeshBasicMaterial,
  TextureLoader,
  BufferGeometry,
  PointsMaterial,
  Points,
  Float32BufferAttribute,
  DirectionalLight,
  Group,
  Color,
} from "three";
import earthUV from "~/assets/earthUV.jpg";
import venusUV from "~/assets/venusUV.jpg";

export default {
  mounted() {
    const globeDiv = document.querySelector("#globeDiv");
    const scene = new Scene();

    let camera = new PerspectiveCamera(
      75,
      globeDiv.offsetWidth / globeDiv.offsetHeight,
      0.1,
      1000
    );

    const renderer = new WebGLRenderer({
      canvas: document.querySelector("canvas"),
      antialias: true,
    });

    renderer.setSize(globeDiv.offsetWidth, globeDiv.offsetHeight);
    renderer.setPixelRatio(devicePixelRatio);

    // Create a sphere
    const sphere = new Mesh(
      new SphereGeometry(5, 50, 50),
      new MeshBasicMaterial({
        map: new TextureLoader().load(venusUV),
      })
    );

    // Create Stars
    const starGeometry = new BufferGeometry();
    const starMaterial = new PointsMaterial({
      color: 0xffffff,
    });

    const starVerticies = [];
    for (let i = 0; i < 10000; i++) {
      const x = (Math.random() - 0.5) * 2000;
      const y = (Math.random() - 0.5) * 2000;
      const z = (Math.random() - 0.5) * 2000;
      starVerticies.push(x, y, z);
    }

    starGeometry.setAttribute(
      "position",
      new Float32BufferAttribute(starVerticies, 3)
    );

    const stars = new Points(starGeometry, starMaterial);

    // Create Lights
    const light = new DirectionalLight(0xffffff, 1);
    light.position.set(0, -1, 1);

    const backLight = new DirectionalLight(0xffffff, 1);
    backLight.position.set(0, 0, -1);

    const group = new Group();
    group.add(sphere);

    // Add objects into scene
    scene.add(light);
    // scene.add(sphere);
    scene.add(backLight);
    scene.add(stars);
    scene.add(group);

    camera.position.z = 15;
    const mouse = {
      x: undefined,
      y: undefined,
    };
    function animate() {
      requestAnimationFrame(animate);
      renderer.render(scene, camera);
      sphere.rotation.y += 0.003;
      sphere.rotation.x += 0.003;
      group.rotation.y = mouse.x * 0.5;
      gsap.to(group.rotation, {
        y: mouse.x * 0.5,
        x: -mouse.y * 0.3,
        duration: 2,
      });
    }
    animate();

    // Animate all divs in.
    gsap.to("#infoDivData", {
      opacity: 1,
      duration: 1.5,
      x: 0,
      ease: "bounce.out",
    });

    addEventListener("mousemove", (event) => {
      mouse.x = (event.clientX / innerWidth) * 2 - 1;
      mouse.y = (event.clientY / innerHeight) * 2 + 1;
    });

    addEventListener("mouseup", (event) => {
      mouse.down = false;
    });

    addEventListener("touchmove", (event) => {
      event.clientX = event.touches[0].clientX;
      event.clientY = event.touches[0].clientY;
      const doesIntersect = raycaster.intersectObject(sphere);
      console.log(doesIntersect);

      mouse.x = (event.clientX / innerWidth) * 2 - 1;
      mouse.y = (event.clientY / innerHeight) * 2 + 1;
    });

    addEventListener("touchend", (event) => {
      mouse.down = false;
    });

    addEventListener("resize", () => {
      renderer.setSize(globeDiv.offsetWidth, globeDiv.offsetHeight);
      camera = new PerspectiveCamera(
        75,
        globeDiv.offsetWidth / globeDiv.offsetHeight,
        0.1,
        1000
      );
      camera.position.z = 15;
    });

    // Transition back to Home Page
    document.querySelector("#homeButton").addEventListener("click", (e) => {
      e.preventDefault();
      scroll({
        top: 700,
        left: 100,
        behavior: "smooth",
      });
      gsap.to(camera.position, {
        ease: "power3.inOut",
        duration: 2,
        z: 25,
      });
      gsap.to(camera.rotation, {
        ease: "power3.inOut",
        duration: 2,
        x: 1.57,
      });
      gsap.to(camera.position, {
        ease: "power3.inOut",
        duration: 2,
        y: -200,
        onComplete: () => {
          gsap.to(camera.position, {
            ease: "power3.in",
            duration: 1.5,
            y: 1000,
            delay: 0.01,
            onComplete: () => {
              this.$router.push("/");
            },
          });
        },
      });
    });
  },
};
</script>
