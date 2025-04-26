<!-- 2025-04-26
Dmytro Manhupli -->
<!-- This is a simple Todo Item component built with Vue 3 and TypeScript. -->
<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'
import { Todo } from '../types/Todo'

const props = defineProps<{
  todo: Todo
}>()

const emit = defineEmits(['toggle', 'remove'])

const handleToggle = () => {
  emit('toggle')
}

const handleRemove = (e: Event) => {
  e.preventDefault()
  emit('remove')
}
</script>

<template>
  <li class="todo-item" :class="{ 'completed': todo.completed }">
    <label>
      <span class="handle">⋮</span>
      <input 
        type="checkbox" 
        :checked="todo.completed" 
        @change="handleToggle"
      />
      <span class="todo-text">{{ todo.text }}</span>
      <a href="#" class="remove" @click="handleRemove">Remove</a>
    </label>
  </li>
</template>

<style scoped>
.todo-item {
  margin: 12px 0;
  padding: 8px 0;
  position: relative;
  transition: all 0.2s ease;
}

label {
  display: flex;
  align-items: center;
  cursor: pointer;
}

.handle {
  cursor: move;
  padding: 0 8px;
  color: var(--color-text-secondary);
  user-select: none;
}

input[type="checkbox"] {
  margin-right: 12px;
  width: 20px;
  height: 20px;
  border-radius: 4px;
  appearance: none;
  border: 2px solid var(--color-primary);
  background-color: transparent;
  position: relative;
  cursor: pointer;
  transition: all 0.2s ease;
}

input[type="checkbox"]:checked {
  background-color: var(--color-primary);
}

input[type="checkbox"]:checked::after {
  content: '✓';
  position: absolute;
  color: white;
  font-size: 14px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.todo-text {
  flex-grow: 1;
  font-size: 16px;
  transition: all 0.2s ease;
}

.completed .todo-text {
  text-decoration: line-through;
  color: var(--color-text-secondary);
}

.remove {
  display: none;
  color: var(--color-error);
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  opacity: 0;
  transition: opacity 0.2s;
}

label:hover .remove {
  display: inline-block;
  opacity: 0.7;
  margin-left: 10px;
}

.remove:hover {
  opacity: 1;
}
</style>