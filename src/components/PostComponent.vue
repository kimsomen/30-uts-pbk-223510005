<template>
  <div class="post-container">
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
</template>

<script>
export default {
  props: {
    userPosts: {
      type: Array,
      required: true
    },
    users: {
      type: Array,
      required: true
    },
    selectedUser: {
      type: Number,
      required: true
    }
  },
  methods: {
    fetchPosts() {
      // Implementation of fetchPosts method
    }
  }
}
</script>

