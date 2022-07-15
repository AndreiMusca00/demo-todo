<script setup>
import TodoListItem from './TodoListItem.vue';
import { ref, reactive } from 'vue';
const response = await fetch('https://jsonplaceholder.typicode.com/todos');


let todos = await response.json();

todos = todos.slice(0, 10)
//o modalitate de a face reactiv elementul - pt array si obiecte simple 
const reactiveTodos = ref(todos);


//a doua modalitate se foloseste cu obiecte ! nu primitive
const counter = reactive({
    deleted: 0,
    updated: 0
});


function handleTodoItemDeleted(todoItemId) {
    console.log("Delete" + todoItemId);
    reactiveTodos.value = reactiveTodos.value.filter(item => item.id !== todoItemId)
    console.log(reactiveTodos.value);
    counter.deleted++;

}
function handleTodoItemCompleted(todoItemId, completed) {
    console.log("Completer" + todoItemId + completed);
    counter.updated++;
}
</script>

<template>
    <p>Changes deleted: {{ counter.deleted }}</p>
    <p>Changes updated: {{ counter.updated }}</p>

    <div class="grid-container">
        <TodoListItem v-for="todo of reactiveTodos" :key="todo.id" :id="todo.id" :title="todo.title"
            :completed="todo.completed" @todo-item-completed="completed => handleTodoItemCompleted(todo.id, completed)"
            @todo-item-deleted="handleTodoItemDeleted(todo.id)" />
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