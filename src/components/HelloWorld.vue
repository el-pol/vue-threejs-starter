<template>
  <div id="container"></div>
</template>

<script>
import * as THREE from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import OrbitControls from "three-orbitcontrols";

export default {
  name: "HelloWorld",
  data() {
    return {
      camera: null,
      controls: null,
      scene: null,
      renderer: null,
      mesh: null,
      hlight: null,
      light: null,
      ligh2: null
    };
  },
  methods: {
    init() {
      let container = document.getElementById("container");
      let loader = new GLTFLoader();
      // loader.crossOrigin = true;

      // Scene
      this.scene = new THREE.Scene();
      // this.scene.background = new THREE.Color(0xdddddd);

      // Camera
      this.camera = new THREE.PerspectiveCamera(
        80,
        window.innerWidth / window.innerHeight,
        0.1,
        800
      );
      this.camera.position.set(5, 5, 5);

      // Lights
      // this.hlight = new THREE.AmbientLight(0x404040, 100);
      // this.scene.add(this.hlight);
      this.light = new THREE.PointLight(0xffffcc, 20, 200);
      this.light.position.set(4, 30, -20);
      this.scene.add(this.light);

      this.light2 = new THREE.AmbientLight(0x20202a, 20, 100);
      this.light2.position.set(30, -10, 30);
      this.scene.add(this.light2);

      // Rendering
      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      // Specific to bug model (https://codepen.io/shshaw/pen/yPPOEg)
      this.renderer.toneMapping = THREE.LinearToneMapping;
      this.renderer.toneMappingExposure = Math.pow(0.94, 5.0);
      this.renderer.shadowMap.enabled = true;
      this.renderer.shadowMap.type = THREE.PCFShadowMap;

      container.appendChild(this.renderer.domElement);

      // Loading model
      loader.load(
        "https://s3-us-west-2.amazonaws.com/s.cdpn.io/39255/ladybug.gltf",
        // We need to use arrow functions otherwise "this" refers to loader and not the Vue instance
        gltf => {
          // this.scene.add(gltf.scene);

          // gltf.animations; // Array<THREE.AnimationClip>
          // gltf.scene; // THREE.Scene
          // gltf.scenes; // Array<THREE.Scene>
          // gltf.cameras; // Array<THREE.Camera>
          // gltf.asset; // Object
          console.log(gltf.scene);
          var object = gltf.scene;
          object.position.set(1, -5, -0.75);
          // let car = gltf.scene.children[0];
          // car.scale.set(0.5, 0.5, 0.5);
          this.scene.add(object);
          this.animate();
        },
        xhr => {
          console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
        },
        error => {
          console.error(error);
        }
      );
      // Controls
      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.rotateSpeed = 0.3;
      this.controls.zoomSpeed = 0.9;

      this.controls.minDistance = 3;
      this.controls.maxDistance = 20;

      this.controls.minPolarAngle = 0; // radians
      this.controls.maxPolarAngle = Math.PI / 2; // radians

      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.05;
    },
    animate() {
      requestAnimationFrame(this.animate);
      // console.log(this.scene)
      this.controls.update();
      this.renderer.render(this.scene, this.camera);
    }
  },
  mounted() {
    this.init();
    this.animate();
  }
};
</script>

<style scoped>
#container {
  width: 100vw;
  height: 100vh;
  margin: 0;
}
</style>
