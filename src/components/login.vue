<template>
	<div id="container"></div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

export default {
	name: "test",
	data() {
		return {
			camera: null, //相机对象
			scene: null, //场景对象
			renderer: null, //渲染器对象
			mesh: null, //网格模型对象Mesh
			controls: null //控件对象
		};
	},
	created() {},
	mounted() {
		this.init();
	},

	methods: {
		init() {
			this.scene = new THREE.Scene();

			/**
			 * 创建网格模型
			 */
			// let geometry = new THREE.SphereGeometry(60, 40, 40); //创建一个球体几何对象
			let geometry = new THREE.BoxGeometry(100, 100, 100); //创建一个立方体几何对象Geometry

			let material = new THREE.MeshLambertMaterial({
				color: "#ffffff"
			}); //材质对象Material
			this.mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
			this.scene.add(this.mesh); //网格模型添加到场景中
			/**
			 * 光源设置
			 */
			//点光源
			let point = new THREE.PointLight("red");
			point.position.set(100, 100, 100); //点光源位置 xyz
			this.scene.add(point); //点光源添加到场景中
			//环境光
			let ambient = new THREE.AmbientLight("black");
			this.scene.add(ambient);
			// console.log(scene)
			// console.log(scene.children)
			/**
			 * 相机设置
			 */
			let container = document.getElementById("container");
			let width = window.innerWidth; //窗口宽度
			let height = window.innerHeight; //窗口高度
			let k = width / height; //窗口宽高比
			let s = 300; //三维场景显示范围控制系数，系数越大，显示的范围越大
			//创建相机对象
			this.camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
			this.camera.position.set(200, 300, 200); //设置相机位置
			this.camera.lookAt(this.scene.position); //设置相机方向(指向的场景对象)
			// this.camera = new THREE.PerspectiveCamera(70, container.clientWidth/container.clientHeight, 0.01, 10);
			//     this.camera.position.z = 1;
			/**
			 * 创建渲染器对象
			 */
			this.renderer = new THREE.WebGLRenderer();
			this.renderer.setSize(width, height); //设置渲染区域尺寸
			this.renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色
			container.appendChild(this.renderer.domElement);

			// let animloop=()=> {
			//   render();
			//   // window.requestAnimationFrame(animloop);
			//   this.controls = new OrbitControls(this.camera,this.renderer.domElement);//创建控件对象
			//   this.controls.addEventListener('change', render);//监听鼠标、键盘事件
			// }
			let render = () => {
				this.renderer.render(this.scene, this.camera); //执行渲染操作
				requestAnimationFrame(render); //请求再次执行渲染函数render
				// this.mesh.rotateY(0.01); //每次绕y轴旋转0.01弧度
			};
			render();
			this.controls = new OrbitControls(this.camera, this.renderer.domElement); //创建控件对象
			// animloop();
			// this.renderer.render(this.scene, this.camera);
			// requestAnimationFrame(()=>{
			//         this.renderer.render(this.scene, this.camera); //执行渲染操作
			//         this.mesh.rotateY(0.01);
			//       }, 20);
		}
	}
};
</script>

<style scoped></style>
