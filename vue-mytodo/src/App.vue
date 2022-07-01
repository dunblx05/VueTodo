<template>
  <div id="app">
    <Modal v-show="showModal" v-on:close="showModal = false">
      <template v-slot:modal-text>{{ modalText }}</template>
    </Modal>
    <TodoHeader></TodoHeader>
    <div v-if="userName">
      <TodoTitle v-bind:propCount="checkCount" v-bind:propName="userName" v-on:changeName="addUserName"></TodoTitle>
      <TodoInput v-on:addItem="addOneItem"></TodoInput>
      <TodoController v-on:clearAll="clearAllItem" v-on:sortItem="sortAllItem"></TodoController>
      <TodoList v-bind:propEmpty="isEmpty" v-bind:propItems="todoItems" v-on:removeItem="removeOneItem" v-on:toggleItem="toggleOneItem"></TodoList>
      <TodoFooter></TodoFooter>
    </div>
    <div v-else>
      <TodoHello v-on:addName="addUserName"></TodoHello>
    </div>
  </div>
</template>

<script>
import Modal from "./components/common/AlertModal"
import TodoHeader from "./components/TodoHeader";
import TodoTitle from "./components/TodoTitle";
import TodoController from "./components/TodoController";
import TodoList from "./components/TodoList";
import TodoFooter from "./components/TodoFooter";
import TodoInput from "./components/TodoInput";
import TodoHello from "./components/TodoHello"
import getDate from "./assets/commonJS/getDate.js";

export default {
  name: 'App',
  data() {
    return {
      userName: '',
      todoItems: [],
      modalText: "",
      showModal: false
    }
  },
  methods: {
    addUserName(userName) {
      localStorage.setItem("userName", userName);
      this.userName = userName;
    },
    addOneItem(todoItem) {
      if (todoItem === "") {
        this.showModal = !this.showModal;
        this.modalText = "The form is empty. Please enter your task.";
        return false;
      }
      for (let i = 0; i < this.todoItems.length; i++) {
        if (this.todoItems[i].item === todoItem) {
          this.showModal = !this.showModal;
          this.modalText = "I think you've already had the task.";
          return false;
        }
      }
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
    },
    isEmpty() {
      return this.todoItems.length <= 0 ? true : false
    }
  },
  created() {
    this.userName = localStorage.getItem("userName")

    if(localStorage.length > 0) {
      for(let i = 0; i < localStorage.length; i++){
        if(localStorage.key(i) !== "loglevel:webpack-dev-server" && localStorage.key(i) !== "userName"){
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
    TodoInput,
    TodoHello,
    Modal
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
