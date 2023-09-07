<template>
  <div class="">
    <h1 id="heading">Todo App</h1>
    <form id="todo-form" @submit.prevent="addTodo">
      <input v-model="newTodo" placeholder="Add a new todo..." maxlength="50">
      <button id="add-button" type="submit">Add Todo</button>
    </form>

    <div class="todo-container">
      <div v-for="todo in todos" :key="todo.id">
        <Transition name="slide-fade">
          <div class="todo" v-if="!todo.removed">
            <div class="check" @click="markTodo(todo)" :class="{ colored: todo.checked }">
            <img v-if="todo.checked" class="img-check" src="./assets/icon-check.svg" alt="">
            </div>
            <p @click="markTodo(todo)" class="todo-title" :class="{ checked: todo.checked }">{{ todo.text }}</p>
            <button @click="removeTodo(todo)" class="delete-button">X</button>
          </div>
          
        </Transition>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      newTodo: '',
    }
  },
  methods: {
    addTodo() {
      if (!this.newTodo) {
        alert("You can't add an empty todo!")
        return
      }

      this.todos.push({ id: id++, text: this.newTodo, checked: false, removed: false })
      localStorage.setItem("Todos", JSON.stringify(this.todos))
      this.newTodo = ''
    },

    removeTodo(todo) {
      todo.removed = true
      setTimeout(() => {
        this.todos = this.todos.filter((t) => t !== todo)
        localStorage.setItem("Todos", JSON.stringify(this.todos))
      }, 500)
      
    },

    markTodo(todo) {
      todo.checked = !todo.checked
      localStorage.setItem("Todos", JSON.stringify(this.todos))
    }
  },
  mounted() {
    if (localStorage.getItem("Todos")) {
      this.todos = JSON.parse(localStorage.getItem("Todos"))
    } else {
      localStorage.setItem("Todos", JSON.stringify(this.todos))
    }
  }
}
let id = 0


</script>



<style scoped>
.todo-container,
.todo-form {
  margin-top: 10px;
  box-shadow: -5px 10px 20px 10px hsla(0, 0%, 6%, .4);
}


#heading {
  text-align: center;
  color: white;
  font-weight: bold;
}

.todo-title {
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  width: 70%;
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

.check {
  border: rgba(255, 255, 255, 0.2) solid 1px;
  border-radius: 50%;
  box-sizing: content-box;
  cursor: pointer;
  outline: none !important;
  width: 1.5rem;
  height: 1.5rem;
}

.img-check {
  width: 0.6rem;
  height: 0.6rem;
}

.colored {
  background: linear-gradient(150deg, #84cf6a, #16c0b0);
  display: flex;
  place-items: center;
  justify-content: center;
}

.checked {
  text-decoration: line-through;
}

#add-button {
  width: 100%;
  font-size: large;
  border-radius: 6px;
  padding: 5px 20px;
  margin-bottom: 10px;
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  color: white;
  box-shadow: 1px 1px 5px rgba(0, 0, 0, 0.57);
  border: none;
  cursor: pointer;
}

.todo {
  display: flex;
  justify-content: space-between;
  background-color: #25273c;
  color: white;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 6px;
  font-size: large;
}

.delete-button {
  background: none;
  border: none;
  padding: 3px 10px;
  border-radius: 20%;
  cursor: pointer;
  color: white;
}

.delete-button:hover {
  background-color: #434557;
  transition: background-color 500ms;
}

@media only screen and (max-width: 750px) {
  .todo {
    font-size: medium;
  }
}
.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(100px);
  opacity: 0;
}
</style>
