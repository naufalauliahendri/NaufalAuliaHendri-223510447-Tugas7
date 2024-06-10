<template>
  <div id="app">
    <div class="container">
      <h1 class="title">Todo List</h1>
      <div class="input-container">
        <input v-model="newTodo" @keyup.enter="addNewTodo" placeholder="Add a new todo" class="todo-input" />
        <button @click="addNewTodo" class="add-button">Add</button>
      </div>
      <ul class="todo-list">
        <li v-for="(todo, index) in todos" :key="index" class="todo-item">
          <input type="checkbox" v-model="todo.done" @change="toggleTodoStatus(index)" />
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
          <button @click="removeTodoItem(index)" class="remove-button">Remove</button>
        </li>
      </ul>
      <p class="unfinished-count">Unfinished Tasks: {{ unfinishedCount }}</p>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import { useTodoStore } from './stores/todoStore';

export default {
  setup() {
    const store = useTodoStore();
    const newTodo = ref('');

    const addNewTodo = () => {
      if (newTodo.value.trim()) {
        store.addTodo(newTodo.value);
        newTodo.value = '';
      }
    };

    const removeTodoItem = (index) => {
      store.removeTodo(index);
    };

    const toggleTodoStatus = (index) => {
      store.toggleTodo(index);
    };

    return {
      newTodo,
      addNewTodo,
      removeTodoItem,
      toggleTodoStatus,
      todos: store.todos,
      unfinishedCount: store.unfinishedCount,
    };
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f0f0f0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app {
  width: 100%;
  max-width: 600px;
  margin: 20px;
  padding: 20px;
  background: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title {
  font-size: 2em;
  margin-bottom: 20px;
  color: #333;
}

.input-container {
  display: flex;
  width: 100%;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 10px;
  font-size: 1em;
  border: 1px solid #ccc;
  border-radius: 5px 0 0 5px;
  outline: none;
}

.add-button {
  padding: 10px 20px;
  font-size: 1em;
  border: none;
  background: #007bff;
  color: #fff;
  cursor: pointer;
  border-radius: 0 5px 5px 0;
  transition: background 0.3s;
}

.add-button:hover {
  background: #0056b3;
}

.todo-list {
  list-style: none;
  width: 100%;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-item input[type="checkbox"] {
  margin-right: 10px;
}

.todo-item .done {
  text-decoration: line-through;
  color: #888;
}

.remove-button {
  margin-left: auto;
  padding: 5px 10px;
  font-size: 0.8em;
  border: none;
  background: #ff4d4d;
  color: #fff;
  cursor: pointer;
  border-radius: 5px;
  transition: background 0.3s;
}

.remove-button:hover {
  background: #cc0000;
}

.unfinished-count {
  margin-top: 20px;
  font-size: 1em;
  color: #333;
}
</style>
