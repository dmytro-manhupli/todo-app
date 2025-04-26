<!-- 2025-04-26
Dmytro Manhupli -->
<!-- This is a simple Todo app built with Vue 3 and TypeScript. It allows users to add, remove, and toggle the completion status of todos. The todos are stored in local storage, and the app features drag-and-drop functionality for reordering the todos. -->
<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue'
import TodoItem from './TodoItem.vue'
import TodoInput from './TodoInput.vue'
import { Todo } from '../types/Todo'
import draggable from 'vuedraggable'

// Initialize todos
const todos = ref<Todo[]>([])

// Local Storage Key
const STORAGE_KEY = 'vue-todo-app'

// Load todos from local storage
onMounted(() => {
  const storedTodos = localStorage.getItem(STORAGE_KEY)
  if (storedTodos) {
    todos.value = JSON.parse(storedTodos)
  } else {
    // Initialize with example todos if no stored todos
    todos.value = [
      { id: 1, text: "Learn about Vue", completed: false },
      { id: 2, text: "Learn about Fliplet", completed: false },
      { id: 3, text: "Play around in JSFiddle", completed: false },
      { id: 4, text: "Show us what you've got", completed: false },
      { id: 5, text: "Send job application", completed: true }
    ]
  }
})

// Save todos to local storage when they change
watch(todos, (newTodos) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(newTodos))
}, { deep: true })

// Computed properties for active and completed todos with getters and setters
const activeTodos = computed({
  get: () => todos.value.filter(todo => !todo.completed),
  set: (value) => {
    const updated = todos.value.map(todo => {
      const moved = value.find(v => v.id === todo.id);
      return moved ? { ...todo, completed: false } : todo;
    });
    todos.value = updated;
  }
})

const completedTodos = computed({
  get: () => todos.value.filter(todo => todo.completed),
  set: (value) => {
    const updated = todos.value.map(todo => {
      const moved = value.find(v => v.id === todo.id);
      return moved ? { ...todo, completed: true } : todo;
    });
    todos.value = updated;
  }
})

// Methods
const addTodo = (text: string) => {
  if (text.trim()) {
    const newId = todos.value.length ? Math.max(...todos.value.map(t => t.id)) + 1 : 1
    todos.value.push({
      id: newId,
      text: text.trim(),
      completed: false
    })
  }
}

const toggleTodo = (id: number) => {
  const todo = todos.value.find(todo => todo.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const removeTodo = (id: number) => {
  const index = todos.value.findIndex(todo => todo.id === id)
  if (index !== -1) {
    todos.value.splice(index, 1)
  }
}
</script>

<template>
  <div class="todo-app">
    <h1>Todo App</h1>
    
    <TodoInput @add-todo="addTodo" />
    
    <div class="todo-section">
      <h2>To do: ({{ activeTodos.length }})</h2>
      <draggable 
        v-model="activeTodos" 
        tag="ol"
        :component-data="{
          type: 'transition-group',
          name: 'list'
        }"
        class="todo-list active-list"
        handle=".handle"
        :group="{ name: 'todos' }"
        item-key="id"
      >
        <template #item="{ element: todo }">
          <TodoItem 
            :todo="todo"
            @toggle="toggleTodo(todo.id)"
            @remove="removeTodo(todo.id)"
          />
        </template>
      </draggable>
    </div>
    
    <hr />
    
    <div class="todo-section">
      <h2>Completed items: ({{ completedTodos.length }})</h2>
      <draggable 
        v-model="completedTodos" 
        tag="ol"
        :component-data="{
          type: 'transition-group',
          name: 'list'
        }"
        class="todo-list completed-list"
        handle=".handle"
        :group="{ name: 'todos' }"
        item-key="id"
      >
        <template #item="{ element: todo }">
          <TodoItem 
            :todo="todo"
            @toggle="toggleTodo(todo.id)"
            @remove="removeTodo(todo.id)"
          />
        </template>
      </draggable>
    </div>
  </div>
</template>

<style scoped>
.todo-app {
  background: var(--color-card-background);
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  transition: all 0.3s ease;
}

h1 {
  font-size: 2em;
  margin-bottom: 1em;
  font-weight: 600;
  color: var(--color-primary);
  text-align: center;
}

h2 {
  font-weight: 600;
  margin-bottom: 12px;
  font-size: 1.2em;
  color: var(--color-text);
}

.todo-section {
  margin-bottom: 24px;
}

.todo-list {
  list-style-position: inside;
  padding-left: 0;
  min-height: 50px;
}

hr {
  border: none;
  height: 1px;
  background-color: var(--color-border);
  margin: 24px 0;
}

/* List transition animations */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from {
  opacity: 0;
  transform: translateX(-20px);
}

.list-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

@media (max-width: 768px) {
  .todo-app {
    padding: 16px;
    border-radius: 8px;
  }
  
  h1 {
    font-size: 1.8em;
    margin-bottom: 0.8em;
  }
}
</style>