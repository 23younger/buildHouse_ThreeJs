<template>
  <canvas id="draw"></canvas>
</template>

<script setup>
import * as THREE from "three";
import { onMounted } from "vue";
import cao from './assets/cao.jpeg';
import { OrbitControls } from './controls/OrbitControls';
import { GLTFLoader } from './loaders/GLTFLoader'

onMounted(() => {
  document.title = '与菲菲看星空';
  let width;
  let height;

  let renderer;
  function initRenderer() {
    width = window.innerWidth;
    height = window.innerHeight;
    const canvas = document.querySelector("#draw");
    renderer = new THREE.WebGLRenderer({
      canvas,
      antialias: true,
      alpha: true,
    })
    renderer.setSize(width, height);
    // renderer.setClearColor(0xffffff, 1.0);
    // document.body.appendChild(renderer.domElement);
  }

  let scene;
  function initScene() {
    scene = new THREE.Scene();
  }

  let camera;
  function initCamera() {
    let k = width / height;
    camera = new THREE.PerspectiveCamera(100, k, 0.1, 1500);
    camera.position.set(8, 1, 12);
    camera.up.x = 0;
    camera.up.y = 1;
    camera.up.z = 0;
    camera.lookAt(scene.position);
  }

  let light;
  function initLight() {
    light = new THREE.AmbientLight(0x404040, 3);
    let pointLight = new THREE.PointLight(0xffffff, 1, 0);
    pointLight.position.set(200, 200, 200); //设置点光源位置
    scene.add(pointLight); //将点光源添加至场景
    scene.add(light);
  }

  let mesh;
  function initObject() {
    const floor = new THREE.PlaneGeometry(1500, 1500);
    const floorTexture = new THREE.TextureLoader().load(cao);
    // floorTexture.repeat.set(1500, 1500);
    const floorMaterial = new THREE.MeshBasicMaterial({ map: floorTexture });
    const floorMesh = new THREE.Mesh(floor, floorMaterial);
    floorMesh.rotation.x = Math.PI * -0.5;
    floorMesh.rotation.z = Math.PI * -0.5;
    scene.add(floorMesh)
  }

  let houseModel1, houseModel2, treeModel;
  function initModel() {
    let house1 = loadModel('src/assets/RobotExpressive.glb').then(res => {
      houseModel1 = res
    })
    let house2 = loadModel('src/assets/model_House.glb').then(res => {
      houseModel2 = res
    })
    let tree = loadModel('src/assets/model_Tree.glb').then(res => {
      treeModel = res;
    })
    Promise.all([house1, house2, tree]).then(() => {
      // 房子1
      // houseModel1.scene.position.set(0, 0, 0);
      // houseModel1.scene.scale.set(20, 20, 20);
      scene.add(houseModel1.scene);
      // 房子2
      houseModel2.scene.position.set(5, 0, 5);
      // houseModel2.scene.scale.set(7, 7, 7);
      // houseModel2.scene.rotation.y = 90;
      scene.add(houseModel2.scene);
      // 树
      let point = randomPoint(10, 100);
      for (let i = 0; i < point.length; i++) {
        // 克隆模型
        let newModel = treeModel.scene.clone();
        let scaleNum = Math.random() * 20;
        newModel.scale.set(scaleNum, scaleNum, scaleNum);
        newModel.position.set(...point[i])
        scene.add(newModel);
      }

      render();
    })
  }

  function loadModel(path) {
    return new Promise((resolve, reject) => {
      const loader = new GLTFLoader();
      loader.load(path, resolve);
    })
  }

  function randomPoint(num, range) {
    let arr = [];
    for (let i = 0; i < num; i++) {
      arr.push([
        Math.random() * range - range / 2 - i,
        0,
        Math.random() * range - range / 2 - i,
      ])
    }
    return arr
  }

  let controls;
  function initControls() {
    controls = new OrbitControls(camera, renderer.domElement);
    const axesHelper = new THREE.AxesHelper(250);
    scene.add(axesHelper);
  }

  function render() {
    requestAnimationFrame( render );
	  renderer.render( scene, camera );
    // renderer.clear();
    // renderer.render(scene, camera);
  }

  function start() {
    initRenderer();
    initScene();
    initLight();
    initCamera();
    initObject();
    initControls();
    initModel()
  }

  start();
})
</script>

<style scoped></style>
