<template>
  <div>
    <div v-if="!loading">

    <header>
      <div class="button-container">
        <button @click="showPosts">Post</button>
        <button @click="showTodos">Todos</button>
      </div>
    </header>
        
    <main>
      <div v-if="selectedMenu === 'posts'" class="post-container">
        <select v-model="selectedUser" @change="fetchPosts">
          <option v-for="user in users" :value="user.id">{{ user.name }}</option>
        </select>
        <div class="table-container">
          <table class="post-table">
            <thead>
              <tr>
                <th>Title</th>
                <th>Fill</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="post in userPosts" :key="post.id">
                <td><strong>{{ post.title }}</strong></td>
                <td>{{ post.body }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  
      <div v-else-if="selectedMenu === 'todos'">
        <div class="container">
          <h1 style="text-align: center;">Task Manager</h1>
          <div style="display: flex;">
            <input type="text" v-model="newTask" placeholder="Add new task...">
            <button @click="addTask">Add</button>
          </div>
  
          <div v-for="(task, index) in incompleteTasks" :key="index" class="task" :class="{ 'completed': task.completed }">
            <input type="checkbox" v-model="task.completed" class="checkbox">
            <div v-if="!task.editing" class="task-text">{{ task.title }}</div>
            <div v-else class="edit-mode">
              <input v-model="task.title" class="edit-input">
              <div class="button-group">
                <button class="update-btn" @click="updateTask(index)">Update</button>
                <button class="cancel-btn" @click="cancelTask(index)">Cancel</button>
              </div>
            </div>
            <div v-if="!task.editing" class="button-group">
              <button class="edit-btn" @click="editTask(index)">Edit</button>
              <button class="delete-btn" @click="deleteTask(index)">Delete</button>
            </div>
          </div>
  
          <button class="filter-btn" @click="showIncomplete = !showIncomplete">
            {{ showIncomplete ? 'Show All Tasks' : 'Show Incomplete Tasks Only' }}
          </button>
        </div>
      </div>
    </main>

    </div>
         <div v-if="loading" class="loading">
      <div class="spinner">
        <div class="double-bounce1"></div>
        <div class="double-bounce2"></div>
      </div>
    </div>
      <div v-else>
      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedMenu: 'posts',
      todos: [],
      users: [],
      selectedUser: null,
      userPosts: [],
      tasks: [],
      newTask: '',
      showIncomplete: true,
      loading: true
    };
  },
  methods: {
    async fetchUsers() {
      this.loading = true;
      const response = await fetch('https://jsonplaceholder.typicode.com/users');
      this.users = await response.json();
      this.loading = false;
    },
    async fetchPosts() {
      this.loading = true;
      const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
      this.userPosts = await response.json();
      this.loading = false;
    },
    showTodos() {
      this.selectedMenu = 'todos';
    },
    showPosts() {
      this.selectedMenu = 'posts';
    },
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ title: this.newTask, completed: false, editing: false });
        this.newTask = '';
      }
    },
    cancelTask(index) {
      this.tasks[index].editing = false;
    },
    editTask(index) {
      this.tasks[index].editing = true;
    },
    updateTask(index) {
      this.tasks[index].editing = false;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    fetchData() {
      setTimeout(() => {
        this.loading = false;
      }, 1000);
    }
  },
  mounted() {
    this.fetchUsers();
    setTimeout(() => {
      this.loading = false;
    }, 1000);
  },
  computed: {
    incompleteTasks() {
      if (this.showIncomplete) {
        return this.tasks.filter(task => !task.completed);
      } else {
        return this.tasks;
      }
    }
  },
  watch: {
    selectedUser() {
      this.fetchPosts();
    },
  }
};
</script>

<style>
* {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

body {
  background-color: #ffffff;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: rgb(0, 0, 0);
}

h1 {
  color: rgb(0, 0, 0);
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

.container {
  width: 100%;
  width: 600px;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0px 0px 10px #37005c;
  background-color: #29292b;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  margin-top: 50px;
}

input[type="text"] {
  width: calc(100% - 95px);
  border: 1px solid #ffffff;
  border-radius: 5px 0 0 5px;
  outline: none;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

button {
  padding: 5px 10px;
  margin: 0 5px;
  background-color: #0c0c0f;
  color: rgb(255, 255, 255);
  border: none;
  border-radius: 5px;
  cursor: pointer;
  outline: none;
}

button:hover {
  background-color: #000000;
}

.task {
  display: flex;
  align-items: center;
  margin: 10px 0;
  padding: 10px;
  border-radius: 5px;
  background-color: rgb(255, 255, 255);
  color: #0c0c0f;
}

.task input[type="checkbox"] {
  margin-right: 10px;
  color: #18181a;
}

.task-text {
  flex: 1;
  cursor: pointer;
  color: #18181a;
  font-weight: bold;
}

.edit-input {
  flex: 1;
  margin-right: 10px;
}

.edit-mode {
  display: flex;
  align-items: center;
}

.button-group {
  display: flex;
  align-items: center;
}

.edit-btn,
.delete-btn,
.cancel-btn {
  margin-left: 5px;
}

.update-btn {
  margin-right: 5px;
}

.completed .task-text {
  text-decoration: line-through;
  opacity: 0.6;
}

.filter-btn {
  margin-top: 200px;
  padding: 10px 15px;
  background-color: #0c0c0f;
  color: rgb(255, 255, 255);
  border: none;
  border-radius: 5px;
  cursor: pointer;
  outline: none;
}

.filter-btn:hover {
  background-color: #0000ff;
}

.edit-input {
  width: calc(100% - 80px);
  margin-right: 10px;
}

.checkbox {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  width: 15px;
  height: 15px;
  border: 5px solid #ffffff;
  border-radius: 5px;
  margin-right: 5px;
}

.checkbox:checked {
  background-color: #1900ff;
}

.button-container {
  text-align: center;
  margin-top: 300px;
}

.button-container button {
  padding: 50px 30px;
  background-color: rgb(255, 255, 255);
  color: #0c0c0f;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 10px;
}

.button-container button:last-child {
  margin-right: 0;
}

.button-container button:hover {
  background-color: #004cff;
}

h2 {
  margin-top: 20px;
  font-size: 24px;
  color: rgb(0, 0, 0);
}

select {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  margin-top: 10px;
  margin-bottom: 5px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  padding: 10px 0;
}

li:last-child {
  border-bottom: none;
}

.post-container {
  margin-top: 20px;
  text-align: center;
}

.table-container {
  margin: 0 auto;
  width: 75%;
  margin-bottom: 500px;
}

.post-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

.post-table th, .post-table td {
  border: 1px solid #ffffff;
  padding: 8px;
}

.post-table th {
  background-color: #0c0c0f;
  color: rgb(255, 255, 255);
}

.post-table td {
  text-align: left;
}

.loading {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 9999;
}

.icons {
  display: flex;
  position: relative;
}

.loading i {
  font-size: 3em;
  color: transparent;
  background: linear-gradient(45deg, #000000,#000000);
  background-clip: text;
  -webkit-background-clip: text;
  animation: up 1s ease infinite;
}

@keyframes up {

  0%,
  100% {
    transform: scale(1);
    opacity: 0.6;
  }

  50% {
    transform: scale(1.5);
    opacity: 1;
  }
}

.loading i:nth-child(2){
  margin-left: -14px;
  animation: up2 1s ease infinite;
}

@keyframes up2 {

  0%,
  100% {
    transform: scale(1.5);
    opacity: 1;

  }

  50% {
    transform: scale(1);
    opacity: 0.6;

  }
}

.icons {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Set the height of the loading container */
}

.loading i {
  font-size: 3em;
  color: transparent;
  background: linear-gradient(45deg, #ffffff,#bdbdbd);
  background-clip: text;
  -webkit-background-clip: text;
  animation: up 1s ease infinite;
}

@keyframes up {

  0%,
  100% {
    transform: scale(1);
    opacity: 0.6;
  }

  50% {
    transform: scale(1.5);
    opacity: 1;
  }
}

.loading i:nth-child(2){
  margin-left: -14px;
  animation: up2 1s ease infinite;
}

@keyframes up2 {

  0%,
  100% {
    transform: scale(1.5);
    opacity: 1;

  }

  50% {
    transform: scale(1);
    opacity: 0.6;

  }
}

@media (max-width: 768px) {
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: #0c0c0f;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: 'Courier New', Courier, monospace;
    }

    .container {
      width: 300px;
    }
  
    input[type="text"] {
      width: calc(100% - 85px);
    }
  }
</style>