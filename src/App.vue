<template>
  <div id="app">
    <ConwayGame v-bind:size="[10, 10]" />
  </div>
</template>

<script>
import ConwayGame from "./components/ConwayGame.vue";

export default {
  name: "App",
  components: {
    ConwayGame
  }
};
</script>

<style lang="scss">
body {
  background-color: #fff0c8;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  background-color: #390f70;
  border: none;
  color: #fff0c8;
  font-weight: bold;
  padding: .5em 1em;
  cursor: pointer;

  &:hover {
    color: white;
  }
}
label {
  font-weight: bold;
}
</style>
