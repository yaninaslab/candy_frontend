<template>
  <div id="app">
    <h1>Welcome to the World of Chocolate</h1>
    <candy-list></candy-list>
  </div>
</template>

<script>
import CandyList from "./components/CandyList.vue";

export default {
  name: "App",
  components: {
    CandyList,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1 {
  color: goldenrod;
  text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;
}
body {
  background-color: rgb(251, 250, 238);
}
</style>
