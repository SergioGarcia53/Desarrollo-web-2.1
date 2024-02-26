<template>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="todos"
        :items-per-page="5"
        class="elevation-1"
      >
        
      </v-data-table>
    </v-container>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const todos = ref([]);
  const headers = [
    { text: 'ID', align: 'start', sortable: true, value: 'id' },
    { text: 'Title', value: 'title' },
    { text: 'Completed', value: 'completed' }
  ];
  
  const fetchTodos = async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      todos.value = await response.json();
    } catch (error) {
      console.error('Error fetching todos:', error);
    }
  };
  
  onMounted(fetchTodos);
  </script>
  
 
  

  