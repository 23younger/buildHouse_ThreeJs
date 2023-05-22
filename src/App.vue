<template>
  <div class="current-page">
    <canvas
      id="draw"
      style="border: 1px solid; background-color: #000;"
    ></canvas>
  </div>
</template>

<script setup>
  import * as THREE from 'three';
  import { onMounted } from 'vue';
  
  // canvas大小
  const width = 500;
  const height = 500;
  // 建筑的长宽
  const baseWidth = 40;
  const baseLength = 60;

  let scene = null; // 场景
  let camera = null; // 相机
  let canvas = null; // 用作渲染器的canvas
  let renderer = null; // 渲染器
</script>

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
