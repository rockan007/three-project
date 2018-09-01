<template>
  <div id="three" class="hello position-relative">
    <div id="Stats-output" class=""></div>
    <div id="webGL-output"></div>
  </div>
</template>

<script>
import * as THREE from "three";
//显示帧数的库
import Stats from "stats-js/build/stats.min.js";
import * as dat from "dat.gui";
export default {
  name: "chapter1-4",
  props: {
    msg: String
  },
  data: function() {
    return {
      camera: "",
      scene: "",
      renderer: "",
      mesh: "",
      stats: "",
      cube: "",
      sphere: "",
      step: 0,
      controls: {
        rotationSpeed: 0.02,
        bouncingSpeed: 0.03
      }
    };
  },
  mounted: function() {
    this.initResize();
    this.initDatGui();
    this.init();
  },
  methods: {
    initResize: function() {
      window.addEventListener("resize", this.onResize, false);
    },
    onResize: function() {
      console.log("获取当前数据onResize");
      let dom = document.getElementById("three");
      console.log("three的宽度："+dom.clientWidth);
      console.log("three的高度："+dom.clientHeight);
      this.camera.aspect = dom.clientWidth / dom.clientHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(dom.clientWidth, dom.clientHeight);
    },
    initDatGui: function() {
      let gui = new dat.GUI();
      gui.add(this.controls, "rotationSpeed", 0, 0.5);
      gui.add(this.controls, "bouncingSpeed", 0, 0.5);
    },
    initStats: function() {
      let stats = new Stats();
      stats.setMode(0);
      stats.domElement.style.position = "absolute";
      stats.domElement.style.left = "0px";
      stats.domElement.style.top = "0px";
      document.getElementById("Stats-output").appendChild(stats.domElement);
      return stats;
    },
    init: function() {
      this.stats = this.initStats();
      let dom = document.getElementById("three");
      let innerWidth = dom.clientWidth;
      let innerHeight = dom.clientHeight;

      //场景，一个容器，主要用于保存、跟踪所要渲染的物体和使用的光源
      this.scene = new THREE.Scene();
      //摄像机
      this.camera = new THREE.PerspectiveCamera(
        45,
        innerWidth / innerHeight,
        0.1,
        1000
      );
      //渲染器
      this.renderer = new THREE.WebGLRenderer();
      //设置场景的背景颜色
      this.renderer.setClearColor(0xeeeeee, 1.0);
      //设置场景的大小
      this.renderer.setSize(innerWidth, innerHeight);
      //阴影效果
      this.renderer.shadowMap.enabled = true;

      //轴对象
      let axes = new THREE.AxesHelper(20);
      this.scene.add(axes);

      //平面的大小
      let planeGeometry = new THREE.PlaneGeometry(60, 20);
      //平面的材质
      let planeMaterial = new THREE.MeshLambertMaterial({ color: 0xffffff });
      //创建plane对象
      let plane = new THREE.Mesh(planeGeometry, planeMaterial);
      //接受阴影
      plane.receiveShadow = true;

      //设置将平面围绕X轴旋转90°
      plane.rotation.x = -0.5 * Math.PI;
      //定义在场景中的颜色
      plane.position.set(15, 0, 0);
      //将平面添加到场景中
      this.scene.add(plane);

      let cubeGeometry = new THREE.BoxGeometry(4, 4, 4);
      let cubeMaterial = new THREE.MeshLambertMaterial({
        color: 0xff0000
        // wireframe: true//线框
      });
      this.cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
      this.cube.castShadow = true;

      this.cube.position.set(-4, 3, 0);
      this.scene.add(this.cube);

      let sphereGeometry = new THREE.SphereGeometry(4, 20, 20);
      let sphereMeterial = new THREE.MeshLambertMaterial({
        color: 0x7777ff
      });
      this.sphere = new THREE.Mesh(sphereGeometry, sphereMeterial);
      this.sphere.castShadow = true;

      this.sphere.position.set(20, 4, 2);
      this.scene.add(this.sphere);

      //设置相机位置
      this.camera.position.set(-30, 40, 30);
      //设置相机指向
      this.camera.lookAt(this.scene.position);

      let spotLight = new THREE.SpotLight(0xffffff);
      spotLight.position.set(-40, 60, -10);
      spotLight.castShadow = true;

      this.scene.add(spotLight);

      document
        .getElementById("webGL-output")
        .appendChild(this.renderer.domElement);
      // this.renderer.render(this.scene, this.camera);
      this.animate();
    },
    animate: function() {
      this.stats.update();

      this.step += this.controls.bouncingSpeed;
      this.sphere.position.x = 20 + 10 * Math.cos(this.step);
      this.sphere.position.y = 2 + 10 * Math.abs(Math.sin(this.step));
      this.cube.rotation.x += this.controls.rotationSpeed;
      this.cube.rotation.y += this.controls.rotationSpeed;
      this.cube.rotation.z += this.controls.rotationSpeed;
      requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
