<template>
  <div v-if="user">
    <AddTodo @submit="addTodo" />
    <TodoList :todos="user.todos" />
  </div>
</template>

<script>
  import AddTodo from "@/components/AddTodo";
  import TodoList from "@/components/TodoList";
  import axios from "@/plugins/axios"; 

  export default {
    components: {
      AddTodo,
      TodoList,
    },
    data() {
      return {
        todos: [],
      };
    },
    computed: {
      user() {
        return this.$store.state.auth.currentUser;
      }
    },
    created() {
      console.log("API_KEY:", process.env.API_KEY);
    },
    methods: {
      async addTodo(todo) {
        const { data } = await axios.post("/v1/todos", { todo });
        this.$store.dispatch("auth/setUser", {
        ...this.user,
        todos: [...this.user.todos, data]
       });
      },
    },
  };

</script>

<style>
</style>
