<template>
    <li class="list-group-item d-flex justify-content-between align-items-center">
      <div>
        <input
          type="checkbox"
          :checked="todo.done"
          @change="$emit('toggle-done', todo.id)"
        />
        <span :class="{ 'text-decoration-line-through': todo.done }">{{ todo.text }}</span>
      </div>
      <div>
        <button @click="$emit('delete', todo.id)" class="btn btn-danger btn-sm">
          Delete
        </button>
        <button @click="editing = true" v-if="!editing" class="btn btn-secondary btn-sm">
          Edit
        </button>
        <div v-if="editing">
          <input type="text" v-model="editedText" class="form-control" />
          <button @click="updateTodo" class="btn btn-primary btn-sm">
            Save
          </button>
          <button @click="cancelEdit" class="btn btn-secondary btn-sm">
            Cancel
          </button>
        </div>
      </div>
    </li>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const props = defineProps({
    todo: {
      type: Object,
      required: true,
    },
  });
  
  const emit = defineEmits(['delete', 'toggle-done', 'update']);
  
  const editing = ref(false);
  const editedText = ref(props.todo.text);
  
  const updateTodo = () => {
    emit('update', editedText.value);
    editing.value = false;
  };
  
  const cancelEdit = () => {
    editing.value = false;
    editedText.value = props.todo.text;
  };
  </script>
  
  <style scoped>
  .text-decoration-line-through {
    text-decoration: line-through;
  }
  </style>
  