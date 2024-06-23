<template>
    <ul class="list-group list-group-flush">
      <TodoItem
        v-for="(todo, index) in todos"
        :key="index"
        :todo="todo"
        @delete="deleteTodo(index)"
        @toggle-done="toggleDone(index)"
        @update="updateTodo(index, $event)"
      >
        <template v-slot:todo-content>
          {{ todo.activity }}
        </template>
        <template v-slot:edit-buttons>
          <button class="btn btn-outline-primary" v-if="!editing" @click="editTodo">
            <font-awesome-icon icon="edit" />
          </button>
          <button class="btn btn-outline-secondary" v-else @click="cancelEdit">
            <font-awesome-icon icon="times" />
          </button>
          <button class="btn btn-outline-secondary" v-if="editing" @click="saveEdit">
            <font-awesome-icon icon="check" />
          </button>
        </template>
      </TodoItem>
    </ul>
  </template>
  
  
  <script setup>
  import { ref, defineProps, defineEmits } from 'vue';
  import TodoItem from './TodoItem.vue';
  
  const props = defineProps({
    todos: {
      type: Array,
      required: true,
    },
  });
  
  const emit = defineEmits(['delete-todo', 'toggle-done', 'update-todo']);
  
  const newTodo = ref('');
  
  const addTodo = () => {
    // Emit an event to add a new todo
    if (newTodo.value.trim()) {
      emit('add-todo', newTodo.value);
      newTodo.value = '';
    }
  };
  
  const deleteTodo = (index) => {
    emit('delete-todo', index);
  };
  
  const toggleDone = (index) => {
    emit('toggle-done', index);
  };
  
  const updateTodo = (index, text) => {
    emit('update-todo', index, text);
  };
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>
  