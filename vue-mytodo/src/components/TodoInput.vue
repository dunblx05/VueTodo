<template>
    <div class="add">
        <input 
            type="text" 
            class="add__input" 
            placeholder="Enter your task" 
            v-model="newTodoItem"
            v-on:keyup.enter="addTodoItem">
        <button class="add__button" v-on:click="addTodoItem">
            <span class="blind">Add</span>
        </button>
    </div>
</template>

<script>

export default {
    data() {
        return {
            newTodoItem: ""
        }
    },
    methods: {
        addTodoItem() {
            if(this.newtodoItem !== "") {
                this.$emit("addItem", this.newTodoItem);
                this.clearInput();
            }
        },
        clearInput() {
            this.newTodoItem = "";
        }
    }
}
</script>
