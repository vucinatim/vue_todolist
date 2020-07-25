<template>
  <div id="todo-list">
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';

export default {
  name: 'TodoList',
  components: {
    Todos,
    AddTodo
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
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }

  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover {
    background: #666;
  }
</style>
