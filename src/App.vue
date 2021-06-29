<template>
  <div id="app">
    <Dict />
  </div>
</template>

<script>
import Dict from "./components/Dict.vue";

export default {
  name: "App",
  components: {
    Dict
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
