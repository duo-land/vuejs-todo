<template>
<div class="todo-app">
  <input class="todo-text-input" placeholder="Add todo" @keyup.enter="addNewTodoItem"/>
  <div class="todo-list">

    <div class="todo-list-item" v-for="todo in filterTodoItems()" :key="todo.id">
      <div class="list-item">
        <input type="checkbox" :checked="todo.completed" @click="markTodoItemComplete($event, todo)"/>
        <div class="todo-text">{{todo.text}}</div>
        <button @click="deleteTodoItem(todo)">X</button>
      </div>
    </div>

  </div>
  <div class="todo-app-filters">
    <button @click="updateFilter('all')">All</button>
    <button @click="updateFilter('active')">Active</button>
    <button @click="updateFilter('completed')">Completed</button>
  </div>
</div>
</template>

<script>
import {ref} from "vue";

export default {
  name: "TodoApp",
  setup() {
    const todos = ref([])
    const showFilter = ref('all')// all/active/completed

    // get previously saved todo items from localstorage
    const todoItems = localStorage.getItem('todo-items')
    if (todoItems !== null) {
      todos.value = JSON.parse(todoItems)
    }

    const updateFilter = filter => {
      showFilter.value = filter
    }

    const filterTodoItems = () => {
      if (showFilter.value === 'active') {
        return todos.value.filter(t => !t.completed)
      }

      if (showFilter.value === 'completed') {
        return todos.value.filter(t => t.completed)
      }

      return todos.value
    }

    const updateLocalStorageWithTodoList = () => {
      localStorage.setItem('todo-items', JSON.stringify(todos.value))
    }

    const addNewTodoItem = (event) => {
      todos.value.unshift({
        id: ~new Date(),
        text: event.target.value,
        completed: false
      })
      event.target.value = ''

      updateLocalStorageWithTodoList()
    }

    const deleteTodoItem = (todoItem) => {
      todos.value = todos.value.filter(t => t.id !== todoItem.id)
      updateLocalStorageWithTodoList()
    }

    const markTodoItemComplete = (event, todo) => {
      todo.completed = event.target.checked
      updateLocalStorageWithTodoList()
    }

    return {
      updateFilter,
      filterTodoItems,
      markTodoItemComplete,
      addNewTodoItem,
      deleteTodoItem,
      todos
    }
  },
  methods: {}
}
</script>

<style>
.todo-app-filters {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.todo-text {
  flex: 1;
  text-align: left;
}

.todo-list {
  padding: 20px;
}

.todo-list-item {
  width: 100%;
}

.list-item {
  height: 50px;
  background: antiquewhite;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  gap: 10px;
  border-bottom: 1px solid cadetblue;
}

.todo-app {
  background: rosybrown;
  width: 450px;
  margin: 0 auto;
  padding: 20px;
}
.todo-text-input {
  width: 400px;
  height: 40px;
  padding: 0 10px;
}
</style>
