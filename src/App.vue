<script setup>
import { ref } from 'vue'

let id = 0

const newTodo = ref('')
const todos = ref([])

if (localStorage.getItem("Todos")) {
  todos.value = JSON.parse(localStorage.getItem("Todos"))
} else {
  localStorage.setItem("Todos", JSON.stringify(todos.value))
}

function addTodo() {
  if (!newTodo.value) {
    alert("You can't add an empty todo!")
    return
  }
  
  todos.value.push({ id: id++, text: newTodo.value})
  localStorage.setItem("Todos", JSON.stringify(todos.value))
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
  localStorage.setItem("Todos", JSON.stringify(todos.value))
}
</script>

<template>
  <h1 id="heading">Todo App</h1>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button id="add-button">Add Todo</button>    
  </form>

  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{  todo.text }}
      <button @click="removeTodo(todo)" class="delete-button">X</button>
    </li>
  </ul>  
</template>

<style scoped>
  #heading {
    text-align: center;
  }

  input {
    width: 100%;
    padding: 8px 27px;
    margin: 5px 0;
    border: 2px solid #bbb;
    border-radius: 6px;
    font-size: large;
    outline: none;
  }

  ul {
    margin-top: 20px;
    font-size: larger;
  }


  #add-button {
    width: 100%;
    font-size: large;
    border-radius: 6px;
    padding: 5px 10px;
    margin-bottom: 10px;
    background: linear-gradient(-90deg, #84cf6a, #16c0b0);
    color: white;
    box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.57);
    border: none;
    cursor: pointer;
  }

  .delete-button {
    background: linear-gradient(-90deg, #e52222, #833c3c);
    border: none;
    padding: 3px 10px;
    border-radius: 20%;
    cursor: pointer;
  }


</style>
