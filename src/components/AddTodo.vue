<template>
  <div class="card" @click="addTaskFormOpen = true">
    <transition name="fadeUp">
      <p id="open-form-btn" class="text-center" v-if="!addTaskFormOpen">
        Add Task
      </p>
    </transition>

    <transition name="expand">
      <form v-if="addTaskFormOpen" @submit="addTodo">
        <input
          type="text"
          v-model="todoTitle"
          name="todoTitle"
          placeholder="Kaj je novega za opraviti?"
        />
        <textarea
          v-model="todoDescription"
          name="todoDescription"
          placeholder="Opis"
        ></textarea>
        <label for="todoDeadline">Deadline: </label>
        <input
          type="date"
          v-model="todoDeadline"
          name="todoDeadline"
          placeholder="Deadline"
        />
        <input type="submit" value="Add" class="btn" />
      </form>
    </transition>
  </div>
</template>

<script>
  import { v4 as uuidv4 } from "uuid";

  export default {
    name: "AddTodo",
    data() {
      return {
        addTaskFormOpen: false,
        todoTitle: "",
        todoDescription: "",
        todoDeadline: "",
      };
    },
    methods: {
      addTodo(e) {
        e.preventDefault();

        const newTodo = {
          id: uuidv4(),
          title: this.todoTitle,
          description: this.todoDescription,
          completed: false,
          deadline: this.todoDeadline,
          user: "user1",
        };
        // Send new todo up to parent
        this.$emit("add-todo", newTodo);

        // Clears the input field
        this.todoTitle, (this.todoDescription = "");

        this.addTaskFormOpen = false;
      },
    },
  };
</script>

<style lang="scss" scoped>
  .card {
    transition: 0.5;
  }

  .text-center {
    text-align: center;
  }

  form {
    margin: 20px 0px;
    position: relative;
  }

  input[type="text"],
  input[type="date"],
  textarea {
    background-color: #f0f0f0 !important;
  }

  input[type="text"] {
    border: none;
    border-bottom: 1px solid rgb(192, 192, 192);
    width: 100%;
    padding: 2px 10px;
    font-size: 18px;
    margin-bottom: 16px;

    &:focus {
      outline: 0;
    }
  }

  input[type="submit"] {
    position: absolute;
    background-color: #33b5e5;
    right: 0;
    bottom: 0;

    &:hover {
      background-color: darken($color: #33b5e5, $amount: 6%);
    }
  }

  input[type="date"] {
    margin-left: 10px;
    border: none;
    border-bottom: 1px solid rgb(192, 192, 192);
  }

  textarea {
    border: 1px solid rgb(192, 192, 192);
    padding: 10px;
    margin: 10px 0px 20px 0px;
    width: 100%;
  }

  #open-form-btn {
    cursor: pointer;
  }

  .expand-enter-active,
  .expand-leave-active {
    overflow: hidden;
    transition: all 0.3s ease-in-out;
  }

  .expand-enter,
  .expand-leave-to {
    height: 0px;
    padding: 0px;
    margin: 0px;
  }

  .expand-leave,
  .expand-enter-to {
    height: 153px;
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
    max-height: 23px;
  }
</style>
