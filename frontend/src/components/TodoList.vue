<template>
    <div class="todo-list">
      <h2>To-Do List</h2>
      <form @submit.prevent="addTodo">
        <input v-model="newTodo" placeholder="Add a new task..." />
        <button type="submit">Add</button>
      </form>
      <ul>
        <li v-for="todo in todos" :key="todo._id">
          <input type="checkbox" v-model="todo.completed" @change="updateTodo(todo)" />
          <span :style="{ textDecoration: todo.completed ? 'line-through' : 'none' }">{{ todo.text }}</span>
          <button @click="deleteTodo(todo._id)">Delete</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  import axios from 'axios'
  
  const todos = ref([])
  const newTodo = ref('')
  
  // Đổi URL này thành địa chỉ backend của bạn nếu cần
  const API_URL = import.meta.env.VITE_BACKEND_URL || 'http://localhost:3030'
  
  const fetchTodos = async () => {
    const res = await axios.get(`${API_URL}/todos`)
    todos.value = res.data
  }
  
  const addTodo = async () => {
    if (!newTodo.value.trim()) return
    await axios.post(`${API_URL}/todos`, { text: newTodo.value })
    newTodo.value = ''
    fetchTodos()
  }
  
  const updateTodo = async (todo) => {
    await axios.put(`${API_URL}/todos/${todo._id}`, { completed: todo.completed })
    fetchTodos()
  }
  
  const deleteTodo = async (id) => {
    await axios.delete(`${API_URL}/todos/${id}`)
    fetchTodos()
  }
  
  onMounted(fetchTodos)
  </script>
  
  <style scoped>
  .todo-list {
    max-width: 400px;
    margin: 0 auto;
  }
  ul {
    list-style: none;
    padding: 0;
  }
  li {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 8px;
  }
  button {
    margin-left: auto;
  }
  </style>
  