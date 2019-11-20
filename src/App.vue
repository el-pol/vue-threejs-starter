<template>
  <div id="app">
    <ThreeJSContainer />
  </div>
</template>

<script>
import ThreeJSContainer from "./components/ThreeJSContainer.vue";

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
