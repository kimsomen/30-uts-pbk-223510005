<template>
    <div>
        <header>
            <div class="button-container">
                <button @click="showPosts">Post</button>
                <button @click="showTodos">Todos</button>
            </div>
        </header>

        <main>
            <PostComponent v-if="selectedMenu === 'posts'" :userPosts="userPosts" :users="users"
                :selectedUser="selectedUser" />
            <TodosComponent v-else-if="selectedMenu === 'todos'" :tasks="tasks" :newTask="newTask"
                :showIncomplete="showIncomplete" />
        </main>

        <div v-if="loading" class="loading">
            <div class="spinner">
                <div class="double-bounce1"></div>
                <div class="double-bounce2"></div>
            </div>
        </div>
    </div>
</template>

<script>
import PostComponent from './PostComponent.vue';
import TodosComponent from './TodosComponent.vue';

export default {
    components: {
        PostComponent,
        TodosComponent
    },
    data() {
        return {
            selectedMenu: 'posts',
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
            // Implementation of fetchUsers method
        },
        showTodos() {
            this.selectedMenu = 'todos';
        },
        showPosts() {
            this.selectedMenu = 'posts';
        }
        // Other methods
    },
    mounted() {
        // Fetch data on component mount
    }
}
</script>