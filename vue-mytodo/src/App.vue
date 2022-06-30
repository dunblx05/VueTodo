<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoTitle v-bind:propsdata="checkCount"></TodoTitle>
    <TodoInput v-on:addItem="addOneItem"></TodoInput>
    <TodoController v-on:clearAll="clearAllItem" v-on:sortItem="sortAllItem"></TodoController>
    <TodoList v-bind:propsdata="todoItems" v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneItem"></TodoList>
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
import getDate from "./assets/commonJS/getDate.js";

export default {
  name: 'App',
  data() {
    return {
      todoItems: []
    }
  },
  methods: {
    addOneItem(todoItem) {
      var value = {
        item: todoItem,
        date: `${getDate().date} ${getDate().week}`,
        time: getDate().time,
        completed: false
      }
      localStorage.setItem(todoItem, JSON.stringify(value))
      this.todoItems.push(value)
    },
    removeOneItem(todoItem, index) {
      localStorage.removeItem(todoItem.index)
      this.todoItems.splice(index, 1)
    },
    toggleOneItem(todoItem) {
      todoItem.completed = !todoItem.completed
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem))
    },
    clearAllItem(){
      this.todoItems = [];
      localStorage.clear();
    },
    sortTodoLatest() {
      this.todoItems.sort(function(a,b){
        return b.time  - a.time
      })
    },
    sortTodoOldest() {
      this.todoItems.sort(function(a,b){
        return a.time - b.time
      })
    },
    sortAllItem(selectedSort) {
      if(selectedSort.value === "date-desc") {
        this.sortTodoLatest();
      } else if(selectedSort.value === "date-asc") {
        this.sortTodoOldest();
      }
    }
  },
  computed: {
    checkCount() {
      const checkLeftItems = () => {
        let leftCount = 0
        for(let i = 0; i < this.todoItems.length; i++){
          if(this.todoItems[i].completed === false){
            leftCount++
          }
        }
        return leftCount
      }
      const count = {
        total: this.todoItems.length,
        left: checkLeftItems()
      }
      return count;
    }
  },
  created() {
    if(localStorage.length > 0) {
      for(let i = 0; i < localStorage.length; i++){
        if(localStorage.key(i) !== "loglevel:webpack-dev-server"){
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
        }
      }
    }
  },
  mounted(){
    this.sortTodoOldest
  },
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
