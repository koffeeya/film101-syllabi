<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello there!" />
</template>

<script>
import * as d3 from "d3";
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  data() {
    return {
      rawData: [],
    };
  },
  components: {
    HelloWorld,
  },
  created() {
    // Load in the data
    Promise.all([d3.csv("./syllabi.csv", d3.autoType)]).then(([data]) => {
      this.rawData = data;
      console.log(this.rawData);
    });
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
</style>
