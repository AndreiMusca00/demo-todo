<script setup>
import TodoListItem from './TodoListItem.vue';

const response = await fetch('https://jsonplaceholder.typicode.com/todos');


const todos = await response.json();

const nameVar = "def";
</script>

<template>
    <div class="grid-container">
        <TodoListItem 
            v-for="todo of todos"
            :key="todo.id"
            :id="todo.id"
            :title="todo.title"
            :completed="todo.completed"
        />
    </div>
</template>

<style scoped>
.grid-container {
  display: grid;
  width: auto;
  grid-template-columns: auto auto auto;
  justify-items: center;
  padding: 10px;
}
</style>