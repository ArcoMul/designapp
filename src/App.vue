<template>
  <div id="app">
    <div id="toolbar">
    </div>
    <el-canvas />
  </div>
</template>

<script>
import Canvas from "@/components/Canvas.vue";

export default {
  name: "home",
  components: {
    'el-canvas': Canvas,
  }
};
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#toolbar {
  height: 5vh;
  width: 100%;
  background-color: #eee;
}
</style>
