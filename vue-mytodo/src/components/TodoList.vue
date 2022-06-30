<template>
    <ul class="list">
        <li class = "list__item" v-for="todoItem in todoItems" v-bind:key="todoItem">
            <input 
                type="checkbox" 
                v-bind:id="todoItem.item"
                v-bind:checked="todoItem.completed === true"
                v-on:change="toggleComplete(todoItem)"
            >
            <label v-bind:for="todoItem.item" class="list__label">
                <p class="list__text">{{ todoItem.item }}</p>
            </label>
            <p class="list__date">{{ todoItem.date }}</p>
            <button class="list__deleted" v-on:click="removeTodo(todoItem, index)">
                <div class="blind">Delete</div>
            </button>
        </li>
    </ul>
</template>

<script>
export default {
    data() {
        return {
            todoItems: []
        }
    },
    methods: {
        toggleComplete(todoItem) {
            todoItem.completed = !todoItem.completed;
            localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
        },
        removeTodo(todoItem, index){
            localStorage.removeItem(todoItem.item);
            this.todoItems.splice(index, 1);
        }
    },
    created() {
        if(localStorage.length > 0){
            for(let i = 0; i < localStorage.length; i++){
                if(localStorage.key(i) !== "loglevel:webpack-dev-server"){
                    this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
                }
            }
        }
    }
}
</script>