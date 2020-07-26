<template>
  <div class="card" @click="addTaskFormOpen = true">
    <transition name="fadeUp">
      <p
        id="open-form-btn"
        class="text-center hover-react"
        v-if="!addTaskFormOpen"
      >
        Add Task
      </p>
    </transition>

    <transition name="expand">
      <form v-if="addTaskFormOpen" @submit="addTodo">
        <p class="validation-msg" v-if="validationMsg">{{ validationMsg }}</p>
        <input
          type="text"
          v-model="todoTitle"
          name="todoTitle"
          placeholder="Kaj je novega za opraviti?"
          required
        />
        <textarea
          v-model="todoDescription"
          name="todoDescription"
          placeholder="Opis"
        ></textarea>
        <input
          class="smaller-input"
          type="text"
          v-model="todoAssignee"
          name="todoAssignee"
          placeholder="Kdo je zadolžen za nalogo?"
          required
        />
        <label for="todoDeadline">Deadline:</label>
        <input
          type="date"
          v-model="todoDeadline"
          name="todoDeadline"
          placeholder="Deadline"
          required
        />
        <input type="submit" value="Add" class="btn" />
      </form>
    </transition>
  </div>
</template>

<script>
  import { v4 as uuidv4 } from "uuid";
  import moment from "moment";

  export default {
    name: "AddTodo",
    data() {
      return {
        validationMsg: null,
        addTaskFormOpen: false,
        todoTitle: "",
        todoDescription: "",
        todoAssignee: "",
        todoDeadline: "",
      };
    },
    methods: {
      validateInput(todo) {
        if (todo.title.length > 30 || todo.assignee.length > 30)
          return (
            "title or assignee to long! Allowed length is: " +
            30 +
            " character."
          );

        if (todo.description.length > 100)
          return (
            "Description to long! Allowed length is: " + 100 + " characters."
          );

        if (
          /^[a-zA-Z ]+$/.test(todo.title) == false ||
          /^[a-zA-Z ]+$/.test(todo.assignee) == false
        )
          return "Forbiden character used. Use only A-Z letters.";

        if (moment(todo.deadline, "MM/DD/YYYY", true).isValid())
          return "Date format is incorrect. Please use built in date-picker";

        return "OK";
      },
      addTodo(e) {
        e.preventDefault();

        const newTodo = {
          id: uuidv4(),
          title: this.todoTitle,
          description: this.todoDescription,
          completed: false,
          deadline: this.todoDeadline,
          assignee: this.todoAssignee,
        };

        var msg = this.validateInput(newTodo);

        if (msg !== "OK") {
          this.validationMsg = msg;
          return;
        }

        // reset validation message
        this.validationMsg = null;

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

  .hover-react {
    transition: all 0.7 ease-in-out !important;

    &:hover {
      color: #33b5e5;
    }
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
    margin: 10px 0px 15px 0px;
    width: 100%;
  }

  .smaller-input {
    font-size: 12px !important;
    padding: 12px !important;
    margin-bottom: 20px !important;
    border: 1px solid rgb(192, 192, 192) !important;
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
    height: 208px;
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
