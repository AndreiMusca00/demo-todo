<script setup >
defineProps({
    id: Number,
    title: String,
    completed: Boolean
})
const emit = defineEmits(['todoItemDeleted', 'todoItemCompleted']);

function handleClick(event) {
    emit('todoItemDeleted');
}
function handleChange(event) {
    emit('todoItemCompleted', event.target.checked);
}
</script>

<template>
    <input type="checkbox" :checked="completed" @change="handleChange" />
    <p>{{ title }}</p>
    <button @click="$emit('todoItemDeleted')">Delete</button>
    <!-- v-on: sau @ -->
    <!-- se mai poate pentru emitere $emit('todoItemDeleted') in partea html -->
</template>
<style scoped>
p,
input,
button {
    margin: 5px;
}

p {
    border: 3px solid black;
    grid-column: 2;
    border-radius: 30%;
    padding: 5px;
    text-align: center;
    justify-self: stretch;
}

input {
    grid-column: 1;
    height: 20px;
    width: 20px;
}



button {
    font-family: 'Source Sans Pro', sans-serif;
    font-weight: 900;
    padding: 10px 5px;
    font-size: 12px;
    border-radius: 20%;
    position: relative;
    overflow: hidden;
    border: 0;
    cursor: pointer;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    background-color: black;
    color: white;
    transition: 0.5s;
    grid-column: 3;
    width: 80px;
}

button:hover {
    color: black;
    background-color: red;
    cursor: pointer;
}
</style>