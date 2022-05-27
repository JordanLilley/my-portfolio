<template>
  <div class="flex">
    <!-- <div id="infoDiv" class="w-1/2">
      <h1>asas</h1>
    </div>
    <div id="globeDiv" class="w-1/2">
      <canvas id="aboutCanvas"></canvas>
    </div> -->
  </div>
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
    const scene = new Scene();
    
    const camera = new PerspectiveCamera(
      75,
      innerWidth / innerHeight,
      0.1,
      1000
    );

    const renderer = new WebGLRenderer({
      canvas: this.$refs.aboutCanvas,
      antialias: true,
    });
    renderer.setSize(innerWidth, innerHeight);
    renderer.setPixelRatio(devicePixelRatio);
    document.body.appendChild(renderer.domElement);

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
