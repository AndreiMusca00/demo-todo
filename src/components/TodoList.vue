<script setup>
//cream un formular pentru a putea face un insert in todos 
import TodoListItem from './TodoListItem.vue';
import { ref, reactive, watch } from 'vue';
let response = await fetch('https://jsonplaceholder.typicode.com/todos');


let todos = await response.json();

todos = todos.slice(0, 10)
//o modalitate de a face reactiv elementul - pt array si obiecte simple 
const reactiveTodos = ref(todos);


//a doua modalitate se foloseste cu obiecte ! nu primitive
const counter = reactive({
    deleted: 0,
    updated: 0
});
//const counter = ref(0)

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
const text = ref("")
let isDisabled = ref(true)
watch(text, newValue => {
    if (newValue != "") {
        isDisabled.value = false
    } else {
        isDisabled.value = true
    }
})



function AddTodo() {
    console.log(text.value);
    console.log(todos.length);
    let idNew = reactiveTodos.length
    let newTodo = {
        userId: 1,
        id: idNew,
        title: text.value,
        completed: false,
    }
    reactiveTodos.value.push(newTodo)
    console.log(reactiveTodos);

    fetch('https://jsonplaceholder.typicode.com/todos', {
        method: 'POST',
        body: JSON.stringify({
            id: idNew,
            title: text.value,
            userId: 1,
            completed: true
        }),
        headers: {
            'Content-type': 'application/json; charset=UTF-8',
        },
    }).then((response) => response.json()).then((json) => console.log(json));
}


</script>

<template>
    <span>Two-way data binding</span>
    <input type="text" v-model="text" />
    <button @click="AddTodo" :disabled="isDisabled">Add </button>
    <p>Changes updated: {{ counter.updated }}</p>

    <p>Changes deleted: {{ counter.deleted }}</p>

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