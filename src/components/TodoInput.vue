<!-- 2025-04-26
Dmytro Manhupli -->
<!-- This is a simple Todo input component built with Vue 3 and TypeScript. It allows users to add new todos by typing in an input field and pressing the "Add" button or the Enter key. The component emits an event when a new todo is added, which can be handled by a parent component. -->
<script setup lang="ts">
import { ref } from 'vue'

const newTodo = ref('')
const emit = defineEmits(['add-todo'])

const addTodo = () => {
  emit('add-todo', newTodo.value)
  newTodo.value = ''
}

const handleKeydown = (e: KeyboardEvent) => {
  if (e.key === 'Enter') {
    addTodo()
  }
}
</script>

<template>
  <div class="todo-input-container">
    <input 
      type="text" 
      v-model="newTodo"
      placeholder="Type something and press ENTER"
      @keydown="handleKeydown"
      class="todo-input"
    />
    <button 
      class="add-button" 
      @click="addTodo"
      :disabled="!newTodo.trim()"
    >
      Add
    </button>
  </div>
</template>

<style scoped>
.todo-input-container {
  display: flex;
  margin-bottom: 24px;
  gap: 8px;
}

.todo-input {
  flex-grow: 1;
  padding: 12px 16px;
  border-radius: 8px;
  border: 1px solid var(--color-border);
  font-size: 16px;
  transition: all 0.2s ease;
  background-color: var(--color-card-background);
  color: var(--color-text);
}

.todo-input:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 2px rgba(10, 132, 255, 0.2);
}

.add-button {
  padding: 12px 20px;
  border-radius: 8px;
  border: none;
  background-color: var(--color-primary);
  color: white;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.add-button:hover {
  background-color: #007AFF;
  transform: translateY(-1px);
}

.add-button:active {
  transform: translateY(0);
}

.add-button:disabled {
  background-color: var(--color-border);
  cursor: not-allowed;
  transform: none;
}

@media (max-width: 768px) {
  .todo-input {
    padding: 10px 12px;
  }
  
  .add-button {
    padding: 10px 16px;
  }
}
</style>