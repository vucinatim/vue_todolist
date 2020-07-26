<template>
  <div id="todo-list">
    <AddTodo v-on:add-todo="createTodo"/>
    <Todos 
      v-bind:todos="todos"
      v-on:del-todo="deleteTodo"
      v-on:update-todo="updateTodo" />
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';

const STORAGE_KEY = 'todo-storage';

export default {
  name: 'TodoList',
  components: {
    Todos,
    AddTodo,
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
  created() {
    this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
  },
  methods: {
    createTodo(newTodo) {
      this.todos = [...this.todos, newTodo];
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
    },
    updateTodo(updatedTodo) {
      this.todos.forEach((item, i) => { if (item.id == updatedTodo.id) this.todos[i] = updatedTodo; } );
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id != id);
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
    }
  }
}
</script>

<style>

</style>
