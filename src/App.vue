<template>
  <div id="app" class="container">
    <HelloWorld msg="Welcome to Your Vue.js Todo App" />

    <todo-list />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import TodoList from "./components/TodoList";

export default {
  name: "App",
  components: {
    HelloWorld,
    TodoList,
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}
.container {
  max-width: 600px;
  margin: 0 auto;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}
</style>
