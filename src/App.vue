<template>
  <div id="app">
    <ThreeJSContainer />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  components: {
    ThreeJSContainer
  }
};
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>
