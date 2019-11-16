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
      hlight: null
    };
  },
  methods: {
    init() {
      let container = document.getElementById("container");
      let loader = new GLTFLoader();

      // Scene
      this.scene = new Three.Scene();
      this.scene.background = new Three.Color(0xdddddd);

      // Camera
      this.camera = new Three.PerspectiveCamera(
        70,
        container.clientWidth / container.clientHeight,
        0.01,
        10
      );
      this.camera.position.z = 1;

      // Lights
      this.hlight = new Three.AmbientLight(0x404040, 100);
      this.scene.add(this.hlight);

      // Rendering
      this.renderer = new Three.WebGLRenderer({ antialias: true });
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      container.appendChild(this.renderer.domElement);

      // Loading model
      loader.load(
        // resource URL
        "https://s3-us-west-2.amazonaws.com/s.cdpn.io/39255/ladybug.gltf",
        // called when the resource is loaded
        function(gltf) {
          // this.scene.add(gltf.scene);

          // gltf.animations; // Array<THREE.AnimationClip>
          // gltf.scene; // THREE.Scene
          // gltf.scenes; // Array<THREE.Scene>
          // gltf.cameras; // Array<THREE.Camera>
          // gltf.asset; // Object
          console.log(gltf.scene);
          // let car = gltf.scene.children[0];
          // car.scale.set(0.5, 0.5, 0.5);
          this.scene.add(gltf.scene);
          this.animate();
        },
        function(xhr) {
          console.log((xhr.loaded / xhr.total) * 100 + "% loaded");
        },
        function(error) {
          console.log(error, "An error happened");
        }
      );
    },
    animate() {
      requestAnimationFrame(this.animate);
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
