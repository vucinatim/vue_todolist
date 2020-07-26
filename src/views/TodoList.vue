<template>
  <div id="todo-list">
    <div class="list-header">
      <h2
        class="list-title text-center"
        @dblclick="editTitle = true"
        v-if="editTitle === false"
      >
        {{ list.title }}
      </h2>
      <input
        class="text-center"
        type="text"
        v-if="editTitle === true"
        v-model="list.title"
        @blur="updateListTitle"
        @keyup.enter="updateListTitle"
      />
      <i
        @click="deleting = !deleting"
        class="delete-list-btn fa fa-trash"
        aria-hidden="true"
      ></i>
    </div>
    <transition name="fadeUp">
      <div v-if="deleting">
        <p class="text-center">
          Are you sure you want to delete this todo list?
        </p>
        <div class="text-center">
          <button @click="deleteList" class="btn">Delete</button>
          <button @click="deleting = false" class="btn">Cancel</button>
        </div>
      </div>
    </transition>
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

  const STORAGE_KEY = "todo-storage";

  export default {
    name: "TodoList",
    components: {
      Todos,
      AddTodo,
    },
    data() {
      return {
        deleting: false,
        list: {},
        editTitle: false,
      };
    },
    created() {
      var id = this.$route.params.id;
      this.list =
        JSON.parse(localStorage.getItem(STORAGE_KEY)).find(
          (list) => list.id == id
        ) || {};
    },
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
      deleteList() {
        this.$emit("remove-list", this.list.id);
        this.$router.push("/");
      },
    },
  };
</script>

<style lang="scss" scoped>
  .btn {
    margin: 10px;
    background-color: #33b5e5;

    &:hover {
      background-color: darken($color: #33b5e5, $amount: 8%);
    }
  }

  .list-header {
    width: 100%;
    position: relative;

    .delete-list-btn {
      font-size: 1.5em;
      width: 50px;
      color: #27a5d3;
      text-align: center;
      position: absolute;
      top: 50%;
      cursor: pointer;
      right: 0;
      transform: translate(0, -50%);
    }

    .list-title {
      color: #27a5d3 !important;
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
  }

  .fadeUp-enter-active,
  .fadeUp-leave-active {
    transition: all 0.3s ease-in-out;
  }

  .fadeUp-enter,
  .fadeUp-leave-to {
    opacity: 0%;
    max-height: 0px;
  }

  .fadeUp-leave,
  .fadeUp-enter-to {
    opacity: 100%;
    max-height: 73px;
  }
</style>
