<template>
  <div id="container"></div>
</template>

<script>
import * as Three from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";

export default {
  name: "HelloWorld",
  data() {
    return {
      camera: null,
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
      this.scene = new Three.Scene();
      // this.scene.background = new Three.Color(0xdddddd);

      // Camera
      this.camera = new Three.PerspectiveCamera(
        80,
        window.innerWidth / window.innerHeight,
        0.1,
        800
      );
      this.camera.position.set(5, 5, 5);

      // Lights
      // this.hlight = new Three.AmbientLight(0x404040, 100);
      // this.scene.add(this.hlight);
      this.light = new Three.PointLight(0xffffcc, 20, 200);
      this.light.position.set(4, 30, -20);
      this.scene.add(this.light);

      this.light2 = new Three.AmbientLight(0x20202a, 20, 100);
      this.light2.position.set(30, -10, 30);
      this.scene.add(this.light2);

      // Rendering
      this.renderer = new Three.WebGLRenderer({ antialias: true });
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      // Specific to bug model (https://codepen.io/shshaw/pen/yPPOEg)
      this.renderer.toneMapping = Three.LinearToneMapping;
      this.renderer.toneMappingExposure = Math.pow(0.94, 5.0);
      this.renderer.shadowMap.enabled = true;
      this.renderer.shadowMap.type = Three.PCFShadowMap;

      container.appendChild(this.renderer.domElement);

      // Loading model
      loader.load(
        // resource URL
        "https://s3-us-west-2.amazonaws.com/s.cdpn.io/39255/ladybug.gltf",
        // called when the resource is loaded
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
    },
    animate() {
      requestAnimationFrame(this.animate);
      // console.log(this.scene)
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
  width: 50vw;
  height: 50vh;
  margin: 0 auto;
}
</style>
