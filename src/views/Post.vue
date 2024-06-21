<template>
    <div class="post-container">
      <h2>Content Title</h2>
      <div class="select-container">
        <select v-model="selectedUser" @change="fetchPosts" class="custom-select">
          <option disabled value="">Select User</option>
          <option v-for="user in users" :value="user.id" :key="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div class="table-container">
        <table class="post-table">
          <thead>
            <tr>
              <th style="text-align: center;">Title</th>
              <th style="text-align: center;">Content</th>
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
  </template>
  
  <script>
  export default {
    data() {
      return {
        users: [],
        selectedUser: null,
        userPosts: []
      };
    },
    methods: {
      async fetchUsers() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users');
          if (!response.ok) {
            throw new Error('Failed to fetch users');
          }
          this.users = await response.json();
        } catch (error) {
          console.error('Error fetching users:', error);
        }
      },
      async fetchPosts() {
        if (this.selectedUser) {
          try {
            const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
            if (!response.ok) {
              throw new Error('Failed to fetch posts');
            }
            this.userPosts = await response.json();
          } catch (error) {
            console.error('Error fetching posts:', error);
          }
        }
      }
    },
    mounted() {
      this.fetchUsers();
    },
    watch: {
      selectedUser() {
        this.fetchPosts();
      }
    }
  };
  </script>
  
  <style scoped>
  .post-container {
    padding: 20px;
    margin: 0 auto;
    text-align: center; 
  }
  
  h2 {
    margin-bottom: 20px;
  }
  
  .select-container {
    margin-bottom: 20px;
  }
  
  .custom-select {
    padding: 15px;
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid #ccc;
    width: 250px; 
    text-align-last: center; 
  }
  
  .table-container {
    border: 1px solid #ffffff;
    border-radius: 5px;
    overflow: hidden;
    width: 95%; 
    margin: 0 auto; 
    margin-top: 20px;
    background-color: #070505;
  }
  
  .post-table {
    width: 100%;
    border-collapse: collapse;
    background-color: #070505;
    border: 1px solid black;
  }
  
  .post-table tr thead th {
    border: 1px solid black ;
    color: black;
  }
  
  .post-table th, .post-table td {
    padding: 10px;
    border: 1px solid black;
    text-align: left;
    color: black;
    background-color: whitesmoke;
  }
  
  .post-table th {
    background-color: #ffffff;
  }
  
  .post-table tr:nth-child(even) {
    background-color: #070505;
  }
  
  strong {
    color: black;
  }
  </style>