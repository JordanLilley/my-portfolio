<template>
  <body>
    <div class="flex h-screen">
      <div
        id="infoDiv"
        class="w-1/2 flex flex-col justify-center px-8 bg-black"
      >
        <h1 class="text-white text-3xl mb-4 font-exo">GENERIC INFORMATION:</h1>
        <div class="text-gray-400 mb-8 font-play text-2xl">
          <p>My name is Jordan Lilley.</p>
          <p>I am 21 years old and am based in the UK, Dorset.</p>
        </div>

        <h1 class="text-white text-3xl mb-4 font-exo">EDUCATION:</h1>
        <div class="text-gray-400 mb-8 font-play text-2xl">
          <p>I studied Computer Science in High School all the way up to the end Sixth Form.</p>
          <p>I then studied Software Engineering at Bournemouth University for three years.</p>
        </div>

        <h1 class="text-white text-3xl mb-4 font-exo">
          PROGRAMMING LANGUAGES:
        </h1>
        <div class="text-gray-400 mb-8 font-play text-2xl">

          <p>Cascading Style Sheets</p>
          <p>C#</p>
          <p>HyperText Markup Language</p>
          <p>Java</p>
          <p>Javascript </p>
          <p>Python</p>
          <p>SQL and NoSQL</p>
          <p>Visual Basic</p>
        </div>

        <h1 class="text-white text-3xl mb-4 font-exo">
          FRAMEWORKS AND RUNTIME ENVIRONMENTS:
        </h1>
        <div class="text-gray-400 mb-8 font-play text-2xl">
          <p>Node.js</p>
          <p>Nuxt.js</p>
          <p>React </p>
        </div>

        <h1 class="text-white text-3xl mb-4 font-exo">IDE PREFERENCE:</h1>
        <div class="text-gray-400 mb-8 font-play text-2xl">
          <p>Visual Studio Code</p>
        </div>

        <h1 class="text-white text-3xl mb-4 font-exo">WORK EXPERIENCE:</h1>
        <div class="text-gray-400 mb-8 font-play text-2xl">
          <p>3 Years in Retail</p>
        </div>

        <div>
          <a
            href="/"
            class="flex text-white bg-green-600 inline-block px-8 py-4 rounded-full font-play justify-center"
          >
            <b>GO HOME</b>
          </a>
        </div>
      </div>
      <div id="globeDiv" class="w-1/2">
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

export default {
  mounted() {
    const globeDiv = document.querySelector("#globeDiv");
    const scene = new Scene();

    const camera = new PerspectiveCamera(
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
        map: new TextureLoader().load(earthUV),
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

    addEventListener("mousemove", (event) => {
      mouse.x = (event.clientX / innerWidth) * 2 - 1;
      mouse.y = (event.clientY / innerHeight) * 2 + 1;
    });
  },
};
</script>
