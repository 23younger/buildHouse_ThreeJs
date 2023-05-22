<template>
  <canvas id="draw"></canvas>
</template>

<script setup>
import * as THREE from "three";
import { onMounted } from "vue";
import cao from './assets/cao.jpeg';

onMounted(() => {
  let canvas = document.querySelector("#draw"); // 用作渲染器的canvas
  const width = window.innerWidth;
  const height = window.innerHeight;

  let scene = new THREE.Scene(); // 场景

  let camera = new THREE.PerspectiveCamera(100, width / height, 0.1, 1500); // 相机
  camera.position.set(8, 1, 12);
  scene.add(camera);

  const floor = new THREE.PlaneGeometry(1500, 1500);
  const textureLoader = new THREE.TextureLoader();
  
  const floorTexture = textureLoader.load(cao);
  floorTexture.repeat.set(1500, 1500)
  const floorMaterial = new THREE.MeshBasicMaterial({ map: floorTexture });
  const floorMesh = new THREE.Mesh(floor, floorMaterial);
  scene.add(floorMesh);

  // 灯光
  const light = new THREE.AmbientLight(0xffffff, 3); // 光照颜色，光照强度
  scene.add(light);

  const renderer = new THREE.WebGL1Renderer({
    canvas, // 渲染器绘制其输出的canvas
    antialias: true, // 是否抗锯齿
    alpha: true, // canvas是否包含alpha-透明度，默认为false
  }); // 渲染器
  // 设置渲染器大小，这里设置为整个窗口，会自动设置canvas的大小
  renderer.setSize(800, 600);
  render();

  function render() {
    renderer.render(scene, camera);
  }
});
</script>

<style scoped></style>
