<template>
  <div id="todo-list">
    <!-- <div> -->
      <AddTodo v-on:add-todo="addTodo"/>
      <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
    <!-- </div> -->
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
// import Sidebar from '../components/layout/Sidebar';

export default {
  name: 'TodoList',
  components: {
    Todos,
    AddTodo,
    // Sidebar
  },
  data() {
    return {
      todos: [
        {
          id: 1,
          title: "Todo One",
          description: "The todo description 1",
          completed: false,
          deadline: new Date(),
          user: "user1",
        },
        {
          id: 2,
          title: "Todo Two",
          description: "The todo description 2",
          completed: true,
          deadline: new Date(),
          user: "user1",
        },
        {
          id: 3,
          title: "Todo Three",
          description: "The todo description 3",
          completed: false,
          deadline: new Date(),
          user: "user1",
        },
      ]
    }
  },
  methods: {
    addTodo(newTodo) {
      this.todos = [...this.todos, newTodo];
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id != id);
    }
  }
}
</script>

<style>

</style>
