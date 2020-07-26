<template>
  <div id="todo-list">
    <AddTodo v-on:add-todo="createTodo" />
    <Todos
      v-bind:todos="list.todos"
      v-on:del-todo="deleteTodo"
      v-on:update-todo="updateTodo"
    />
  </div>
</template>

<script>
  import Todos from "../components/Todos";
  import AddTodo from "../components/AddTodo";

  export default {
    name: "TodoList",
    components: {
      Todos,
      AddTodo,
    },
    props: ["list"],
    methods: {
      createTodo(newTodo) {
        this.list.todos = [...this.list.todos, newTodo];
        this.$emit("update-list", this.list);
      },
      updateTodo(updatedTodo) {
        this.list.todos.forEach((item, i) => {
          if (item.id == updatedTodo.id) this.list.todos[i] = updatedTodo;
        });
        this.$emit("update-list", this.list);
      },
      deleteTodo(id) {
        this.list.todos = this.list.todos.filter((todo) => todo.id != id);
        this.$emit("update-list", this.list);
      },
    },
  };
</script>

<style></style>
