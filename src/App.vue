<template>
  <div id="app">
    <Todo/>
    <tehnologies/>
  </div>
</template>

<script>
import Todo from "./components/Todo.vue";
import tehnologies from "./components/tehnologies.vue";
export default {
  name: "app",
  components: {
    Todo,
    tehnologies
  }
};
</script>

<style lang="scss">
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
