<template>
  <div id="todo-list">
    <h2
      class="list-title text-center"
      @dblclick="editTitle = true"
      v-if="editTitle === false"
    >{{ list.title }}</h2>
    <input
      class="text-center"
      type="text"
      v-if="editTitle === true"
      v-model="list.title"
      @blur="updateListTitle"
      @keyup.enter="updateListTitle"
    />
    <AddTodo v-on:add-todo="createTodo" />
    <Todos v-bind:todos="list.todos" v-on:del-todo="deleteTodo" v-on:update-todo="updateTodo" />
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
  data() {
    return {
      editTitle: false,
    };
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
    updateListTitle() {
      this.$emit("update-list", this.list);
      this.editTitle = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.list-title {
  color: #33b5e5 !important;
  margin: 10px 0px 30px 0px;
  cursor: pointer;
}

input[type="text"] {
  width: 100%;
  margin: 10px 0px;
  padding: 10px;
  background-color: #f0f0f0 !important;
  border: 1px solid rgb(192, 192, 192);
}
</style>
