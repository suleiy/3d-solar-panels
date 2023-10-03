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
  BoxGeometry,
  MeshBasicMaterial,
  MeshNormalMaterial,
} from "three";
import { onMounted, ref } from "vue";
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";
import { OrbitControls } from "three/addons/controls/OrbitControls.js";
import { DragControls } from "three/addons/controls/DragControls.js";

import { RBGELoader } from "../modules/RGBELoader.js";
import { TTFLoader } from "three/examples/jsm/loaders/TTFLoader";
import { FontLoader } from "three/examples/jsm/loaders/FontLoader";
import { TextGeometry } from "three/examples/jsm/geometries/TextGeometry";

onMounted(() => {
  const experience = ref<HTMLCanvasElement | null>(null);
  const scene = new Scene();

  var bgLoader = new TextureLoader();
  bgLoader.load("src/assets/3d-models/bg.jpeg", function (texture) {
    scene.background = texture;
  });
  let container = document.getElementById("container") as HTMLElement;
  let textArea = document.getElementById("text") as HTMLElement;
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
  spotlight.castShadow = true;
  scene.add(spotlight);

  const directionalLight = new DirectionalLight(0xffffff, 10);
  directionalLight.castShadow = true;
  scene.add(directionalLight);

  // LOAD GLTF
  const loader = new GLTFLoader().setPath("src/assets/3d-models/");
  const textureLoader = new TextureLoader();
  const solarTexture1 = textureLoader.load("src/assets/3d-models/solar.webp");
  const solarTexture2 = textureLoader.load("src/assets/3d-models/black.webp");

  let objects = [] as any;
  const geometry = new BoxGeometry(5.08, 3.29, 0.01);

  const glassMaterial = new MeshBasicMaterial({
    color: 0xfffffff,
    transparent: true,
    opacity: 0.2,
  });

  const translucentMaterial = new MeshBasicMaterial({
    color: 0xfffffff,
    transparent: true,
    opacity: 0.4,
  });

  const cellMaterial = new MeshBasicMaterial({
    color: 0xfffffff,
    map: solarTexture1,
  });
  const cellMaterial2 = new MeshBasicMaterial({
    color: 0xfffffff,
    map: solarTexture2,
  });

  const material = new MeshBasicMaterial({
    color: 0xfffffff,
  });

  // Panel 1

  //tempered glass
  const glass1 = new Mesh(geometry, glassMaterial);
  glass1.position.set(1.4, 1.36, -0.65);
  glass1.rotation.x = -80.1;
  glass1.rotation.y = -0.2;
  objects.push(glass1);
  scene.add(glass1);

  // EVA
  const eva1 = new Mesh(geometry, translucentMaterial);
  eva1.position.set(1.4, 1.38, -0.65);
  eva1.rotation.x = -80.1;
  eva1.rotation.y = -0.2;
  objects.push(eva1);
  scene.add(eva1);

  // solar cells
  const cells1 = new Mesh(geometry, cellMaterial);
  cells1.position.set(1.4, 1.37, -0.65);
  cells1.rotation.x = -80.1;
  cells1.rotation.y = -0.2;
  objects.push(cells1);
  scene.add(cells1);

  // EVA
  const eva2 = new Mesh(geometry, translucentMaterial);
  eva2.position.set(1.4, 1.38, -0.65);
  eva2.rotation.x = -80.1;
  eva2.rotation.y = -0.2;
  objects.push(eva2);
  scene.add(eva2);

  // back sheet
  const back1 = new Mesh(geometry, material);
  back1.position.set(1.4, 1.33, -0.65);
  back1.rotation.x = -80.1;
  back1.rotation.y = -0.2;
  objects.push(back1);
  scene.add(back1);

  // aluminium frame
  loader.load("frame.gltf", (gltf) => {
    const mesh = gltf.scene;
    mesh.position.set(0, 0.02, 0);
    mesh.castShadow = true;
    mesh.receiveShadow = true;
    objects.push(mesh);
    scene.add(mesh);
  });

  /////////// PANEL 2 //////////////////
  loader.load("panel.gltf", (gltf) => {
    const mesh2 = gltf.scene;
    mesh2.position.set(0, 0.02, -5);
    mesh2.castShadow = true;
    mesh2.receiveShadow = true;
    objects.push(mesh2);
    scene.add(mesh2);
  });

  //tempered glass
  const glass2 = new Mesh(geometry, glassMaterial);

  glass2.position.set(1.4, 1.36, -5.65);
  glass2.rotation.x = -80.1;
  glass2.rotation.y = -0.2;
  objects.push(glass2);
  scene.add(glass2);

  // EVA
  const eva3 = new Mesh(geometry, translucentMaterial);

  eva3.position.set(1.4, 1.38, -5.65);
  eva3.rotation.x = -80.1;
  eva3.rotation.y = -0.2;
  objects.push(eva3);
  scene.add(eva3);

  // solar cells
  const cells2 = new Mesh(geometry, cellMaterial2);
  cells2.position.set(1.4, 1.37, -5.65);
  cells2.rotation.x = -80.1;
  cells2.rotation.y = -0.2;
  objects.push(cells2);
  scene.add(cells2);

  // EVA
  const eva4 = new Mesh(geometry, translucentMaterial);
  eva4.position.set(1.4, 1.38, -5.65);
  eva4.rotation.x = -80.1;
  eva4.rotation.y = -0.2;
  objects.push(eva4);
  scene.add(eva4);

  // back sheet
  const back2 = new Mesh(geometry, material);
  back2.position.set(1.4, 1.33, -5.65);
  back2.rotation.x = -80.1;
  back2.rotation.y = -0.2;
  objects.push(back2);
  scene.add(back2);

  loader.load("thinfilm2.gltf", (gltf) => {
    const mesh3 = gltf.scene;

    mesh3.position.set(0, 0, 5);
    mesh3.scale.set(2, 2, 2);
    objects.push(mesh3);
    scene.add(mesh3);
  });
  // let objects = [mesh, mesh2, mesh3];

  const fontLoader = new FontLoader();
  fontLoader.load(
    "node_modules/three/examples/fonts/droid/helvetiker_regular.typeface.json",
    (droidFont) => {
      const poly = new TextGeometry("polycrystalline", {
        height: 0.1,
        size: 0.5,
        font: droidFont,
        bevelEnabled: false,
      });

      const mono = new TextGeometry("monocrystalline", {
        height: 0.1,
        size: 0.5,
        font: droidFont,
        bevelEnabled: false,
      });

      const thin = new TextGeometry("thin-film", {
        height: 0.1,
        size: 0.5,
        font: droidFont,
        bevelEnabled: false,
      });

      const textMaterial = new MeshBasicMaterial({ color: "orange" });

      const polyTextMesh = new Mesh(poly, textMaterial);
      polyTextMesh.rotation.y = -30.02;

      const monoTextMesh = new Mesh(mono, textMaterial);
      monoTextMesh.rotation.y = -30.02;

      const thinTextMesh = new Mesh(thin, textMaterial);
      thinTextMesh.rotation.y = -30.02;

      polyTextMesh.position.set(5, 0.5, 2);
      monoTextMesh.position.set(5, 0.5, -4);
      thinTextMesh.position.set(5, 0.5, 7);
      scene.add(polyTextMesh);
      scene.add(monoTextMesh);
      scene.add(thinTextMesh);
    }
  );

  const renderer = new WebGLRenderer({
    antialias: true,
  });
  const dControls = new DragControls(objects, camera, renderer.domElement);
  dControls.enabled = true;

  dControls.deactivate();
  dControls.activate();
  renderer.shadowMap.enabled = true;

  renderer.setSize(window.outerWidth, window.outerHeight);
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
