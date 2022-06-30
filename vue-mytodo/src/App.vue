<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoTitle></TodoTitle>
    <TodoInput></TodoInput>
    <TodoController></TodoController>
    <TodoList></TodoList>
    <TodoFooter></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader";
import TodoTitle from "./components/TodoTitle";
import TodoController from "./components/TodoController";
import TodoList from "./components/TodoList";
import TodoFooter from "./components/TodoFooter";
import TodoInput from "./components/TodoInput.vue";

export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoTitle,
    TodoController,
    TodoList,
    TodoFooter,
    TodoInput
}
}
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
