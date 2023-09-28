<script setup lang="ts">
import {
  PerspectiveCamera,
  Scene,
  WebGLRenderer,
  PlaneGeometry,
  MeshStandardMaterial,
  DoubleSide,
  Mesh,
  SpotLight,
  TextureLoader,
  DirectionalLight,
  Vector3,
} from "three";
import { onMounted, ref } from "vue";
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";

onMounted(() => {
  const experience = ref<HTMLCanvasElement | null>(null);
  const scene = new Scene();
  let container = document.getElementById("container") as HTMLElement;
  // CAMERA
  const camera = new PerspectiveCamera(
    45,
    window.innerWidth / window.innerHeight,
    1,
    1000
  );
  camera.position.set(4, 5, 11);
  // camera.lookAt(0, 0, 0);
  // TEST OBJ
  const groundGeometry = new PlaneGeometry(20, 20, 32, 32);
  groundGeometry.rotateX(-Math.PI / 2);
  const groundMaterial = new MeshStandardMaterial({
    color: 0x555555,
    side: DoubleSide,
  });
  const groundMesh = new Mesh(groundGeometry, groundMaterial);
  scene.add(groundMesh);

  // SPOTLIGHT
  const spotlight = new SpotLight(0xffffff, 5, 0, 250, 1, 0.2);
  // spotlight.position.set(0, 25, 0);
  scene.add(spotlight);

  const directionalLight = new DirectionalLight(0xffffff, 10);
  scene.add(directionalLight);

  // LOAD GLTF
  const loader = new GLTFLoader().setPath("src/assets/3d-models/");

  loader.load("panel.gltf", (gltf) => {
    const mesh = gltf.scene;
    mesh.position.set(0, 0, 0);
    console.log(mesh.getObjectByName("Cube"));
    scene.add(mesh);
  });
  const renderer = new WebGLRenderer({
    antialias: true,
  });
  renderer.setSize(window.innerWidth, window.innerHeight);
  renderer.setClearColor(0x000000);

  renderer.render(scene, camera);
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.enablePan = false;
  controls.minDistance = 5;
  controls.minPolarAngle = 0.5;
  controls.maxPolarAngle = 1.5;
  controls.autoRotate = false;
  controls.target = new Vector3(0, 1, 0);
  controls.update();

  function animate() {
    requestAnimationFrame(animate);
    controls.update();
    renderer.render(scene, camera);
  }
  animate();
  container.appendChild(renderer.domElement);
});
</script>

<template>
  <div id="container"></div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
