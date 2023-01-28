<template>
  <div class="container" ref="container"></div>
</template>

<script setup>
import * as THREE from 'three';
//轨道控制器
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
//读取hdr文件
import { RGBELoader } from 'three/examples/jsm/loaders/RGBELoader';
import { ref, onMounted } from 'vue';

//初始化场景
const scene = new THREE.Scene();
//初始化相机
const camera = new THREE.PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
);

//设置相机位置
camera.position.z = 5;
//初始化渲染器
const renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);

const container = ref(null);

//渲染函数
const render = () => {
  renderer.render(scene, camera);
  //请求动画帧
  requestAnimationFrame(render);
}

// //添加立方体
// const geometry = new THREE.BoxGeometry(10,10,10);
// //添加材质
// const material = new THREE.MeshBasicMaterial({ color:0x00ff00 });
// const cube = new THREE.Mesh(geometry, material);
// scene.add(cube);

//添加球体
const geometry = new THREE.SphereGeometry(5, 32, 32)
const loader = new RGBELoader();
loader.load('./imgs/hdr/01.hdr', (texture) => {
  const material = new THREE.MeshBasicMaterial({ map:texture });
  const sphere = new THREE.Mesh(geometry, material);
  sphere.geometry.scale(1, 1, -1);
  scene.add(sphere);
})

// //照片数组,左右、上下、前后
// var arr = ['1', '2', '3', '4', '5', '6'];
// var boxMaterial = [];
// arr.forEach((item)=>{
//   //纹理加载
//   let texture = new THREE.TextureLoader().load(`./imgs/${item}.jpeg`);
//   //创建材质
//   boxMaterial.push(new THREE.MeshBasicMaterial({ map:texture }));
// });
// const cube = new THREE.Mesh(geometry, boxMaterial);
// //让z轴反过来，进入内部
// cube.geometry.scale(1, 1, -1)
// scene.add(cube);

//挂载完毕或获取dom
onMounted(() => {
  //添加控制器
  const controls = new OrbitControls(camera, container.value);
  //给控制器添加阻尼
  controls.enableDamping = true;
  container.value.appendChild(renderer.domElement);
  render();
});
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
</style>
