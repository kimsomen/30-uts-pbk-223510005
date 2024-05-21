<template>
  <div class="container">
    <h1 style="text-align: center;">Task Manager</h1>
    <!-- Todos content -->
  </div>
</template>

<script>
export default {
  props: {
    tasks: {
      type: Array,
      required: true
    },
    newTask: {
      type: String,
      required: true
    },
    showIncomplete: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    // Implementation of methods
  }
}
</script>
