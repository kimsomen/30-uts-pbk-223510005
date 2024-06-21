<template>
    <div class="container">
      <h1>Task List</h1>
      <div class="input-container">
        <input type="text" v-model="newTask" placeholder="input new task...">
        <q-btn class="q-btn" @click="addTask" label="Add" />
      </div>
  
      <div v-for="(task, index) in filteredTasks" :key="index" class="task" :class="{ 'completed': task.completed }">
        <div class="task-content">
          <q-checkbox v-model="task.completed" class="checkbox" @change="completeTask(index)" />
          <div v-if="!task.editing" class="task-text">{{ task.title }}</div>
          <div v-else class="edit-mode">
            <q-input v-model="task.title" class="edit-input" dense />
            <div class="button-group">
              <q-btn class="q-btn" @click="updateTask(index)" label="Update" />
              <q-btn class="q-btn" @click="cancelTask(index)" label="Cancel" />
            </div>
          </div>
        </div>
        <div class="button-group">
          <q-btn class="q-btn" v-if="!task.editing" @click="editTask(index)" label="Edit" />
          <q-btn class="q-btn" @click="deleteTask(index)" label="Delete" />
        </div>
      </div>
  
      <q-btn class="filter-btn" @click="toggleShowIncomplete" :label="showIncomplete ? 'Show All Tasks' : 'Show only incomplete tasks'" />
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  import { QBtn, QCheckbox, QInput } from 'quasar';
  
  const tasks = ref([]);
  
  const newTask = ref('');
  const showIncomplete = ref(true);
  
  const addTask = () => {
    if (newTask.value.trim() !== '') {
      tasks.value.push({ title: newTask.value, completed: false, editing: false });
      newTask.value = '';
    }
  };
  
  const cancelTask = (index) => {
    tasks.value[index].editing = false;
  };
  
  const editTask = (index) => {
    tasks.value[index].editing = true;
  };
  
  const updateTask = (index) => {
    tasks.value[index].editing = false;
  };
  
  const deleteTask = (index) => {
    tasks.value.splice(index, 1);
  };
  
  const completeTask = (index) => {
    tasks.value[index].completed = !tasks.value[index].completed;
  };
  
  const toggleShowIncomplete = () => {
    showIncomplete.value = !showIncomplete.value;
  };
  
  const filteredTasks = computed(() => {
    if (showIncomplete.value) {
      return tasks.value.filter(task => !task.completed);
    } else {
      return tasks.value;
    }
  });
  </script>
  
  <style scoped>
  .container {
    border-radius: 30px;
    max-width: 900px;
    margin: 0 auto;
    border: 15px solid #ffff00;
    padding: 20px;
    margin-top: 30px;
    background-color:   #f9f9f9;
  }
  
  h1 {
    text-align: center;
    font-size: 2rem;
    margin-bottom: 20px;
    color: #333;
  }
  
  .input-container {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
  }
  
  input[type="text"] {
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 1rem;
  }
  
  .q-btn {
    padding:20px 20px;
    background-color: #ffff00;
    border: none;
    border-radius: 4px;
    color: #000000;
    cursor: pointer;
    font-size: 1rem;
    transition: background-color 0.3s ease;
  }
  
  .q-btn:hover {
    background-color: #ffff00;
  }
  
  .task {
    border: 5px solid #ffff00;
    border-radius: 20px;
    padding: 15px;
    margin: 20px 0;
    background-color: #f9f9f9;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .completed {
    text-decoration: line-through;
    color: gray;
  }
  
  .task-content {
    display: flex;
    align-items: center;
    gap: 10px;
  }
  
  .button-group {
    display: flex;
    gap: 10px;
    margin-top: 5px;
  }
  
  .filter-btn {
    background-color: #ffff00;
    color: #000000;
    border: none;
    border-radius: 5px;
    padding: 10px 20px;
    cursor: pointer;
    font-size: 1rem;
    margin-top: 20px;
    transition: background-color 0.3s ease;
  }
  
  .filter-btn:hover {
    background-color: #ffffff;
  }
  
  .task-text {
    flex: 1;
    color: #000;
  }
  
  .edit-mode {
    display: flex;
    gap: 10px;
    align-items: center;
  }
  
  .edit-input {
    flex: 1;
  }
  </style>
  