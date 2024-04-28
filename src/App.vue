<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { ref, computed } from 'vue';

const todos = ref([]);

const newTodo = ref('');
const hideCompleted = ref(false);

const addTodo = () => {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ id: Date.now(), text: newTodo.value, done: false });
    newTodo.value = '';
  }
};

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter(todo => !todo.done) : todos.value;
});

const removeTodo = todo => {
  const index = todos.value.indexOf(todo);
  if (index !== -1) {
    todos.value.splice(index, 1);
  }
};
</script>



<template>
  <div class="container mx-auto bg-slate-500 rounded-xl shadow border p-20 m-10">
      <div class="text-center">
          <h1 class="text-3xl font-bold pb-4">List Kegiatan</h1>
      </div>
      <div class="font-bold pb-4">
          <form @submit.prevent="addTodo">
          <input class="shadow-lg border-spacing-2 p-2 rounded-lg " placeholder="Add list" v-model="newTodo">
          <button class="px-6 border rounded-lg bg-slate-700 text-white">Add</button>
      </form>
      </div>
      <ul>
  <li v-for="todo in filteredTodos" :key="todo.id">
    <input type="checkbox" v-model="todo.done">
    <span :class="{ done: todo.done }">{{ todo.text }}</span>
    <button @click="removeTodo(todo)" class="px-3">Delete
      <TrashIcon class="h-6 w-6 text-gray-500" />
    </button>
  </li>
</ul>
<button class="border bg-slate-800 text-white text-base mt-3" @click="hideCompleted = !hideCompleted">
  {{ hideCompleted ? 'Show all' : 'Hide completed' }}
</button>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}

.container {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  background-color: #9900ff35;
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
}

.title {
  text-align: center;
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.input-field {
  flex: 1;
  border: none;
  border-radius: 5px;
  padding: 10px;
  margin-right: 10px;
}

.add-button {
  border: none;
  border-radius: 5px;
  background-color: #8700d5;
  color: white;
  padding: 10px 20px;
  cursor: pointer;
}

.todos-container {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.todo-text {
  flex: 1;
  margin-left: 10px;
  color: #333;
}

.remove-button {
  background-color: #f44336;
  color: rgb(255, 0, 0);
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
}

.remove-button:hover {
  background-color: #d32f2f;
}
</style>