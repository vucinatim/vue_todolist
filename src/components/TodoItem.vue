<template>
  <div class="card">
    <transition name="fade">
      <div
        class="todo-item"
        v-if="!edit"
        v-bind:class="{ 'is-complete': todo.completed }"
      >
        <div class="flex-container">
          <div class="flex-item">
            <input
              type="checkbox"
              v-model="todo.completed"
              @change="$emit('update-todo', todo)"
            />
            <div class="float-right">
              <p>{{ todo.title }}</p>
              <small>{{ todo.description }}</small>
            </div>
          </div>
          <div class="btn-group">
            <div class="m-auto text-center float-left todo-info">
              <p class="deadline-text">
                {{ timeUntilDeadline(todo.deadline) }}
              </p>
              <small class="pre-text">Assigned:</small>
              <small class="assignee-text text-center">{{
                todo.assignee
              }}</small>
            </div>
            <i
              @click="startEditMode"
              class="pointer v-bottom fa fa-pencil-square-o"
              aria-hidden="true"
            ></i>
            <button @click="$emit('del-todo', todo.id)" class="flex-item del">
              &times;
            </button>
          </div>
        </div>
      </div>
    </transition>

    <transition name="expand">
      <div class="todo-item-edit" v-if="edit">
        <form @submit="updateTodo">
          <p class="validation-msg" v-if="validationMsg">{{ validationMsg }}</p>
          <input type="text" v-model="todoTitle" name="todoTitle" required />
          <textarea v-model="todoDescription" name="todoDescription"></textarea>
          <label for="todoAssignee">Assigned person:</label>
          <input
            type="text"
            v-model="todoAssignee"
            name="todoAssignee"
            required
          />
          <label for="todoDeadline">Deadline:</label>
          <input
            type="date"
            v-model="todoDeadline"
            name="todoDeadline"
            required
          />
          <div class="keep-inline">
            <input type="submit" value="Save" class="btn" />
            <input
              type="button"
              @click="endEditMode"
              value="Cancel"
              class="btn"
            />
          </div>
        </form>
      </div>
    </transition>
  </div>
</template>

<script>
  import moment from "moment";

  export default {
    name: "TodoItem",
    props: ["todo"],
    data() {
      return {
        validationMsg: null,
        edit: false,
        todoTitle: this.todo.title,
        todoDescription: this.todo.description,
        todoAssignee: this.todo.assignee,
        todoDeadline:
          new Date(this.todo.deadline).toISOString().split("T")[0] ||
          new Date(),
      };
    },
    methods: {
      timeUntilDeadline(deadline) {
        var diffTime = new Date(deadline) - new Date();
        var diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        if (diffDays > 0) return diffDays + " days left.";
        else return "Deadline reached!";
      },
      updateTodo(e) {
        e.preventDefault();

        this.todo.title = this.todoTitle;
        this.todo.description = this.todoDescription;
        this.todo.assignee = this.todoAssignee;
        this.todo.deadline = this.todoDeadline;

        var msg = this.validateInput(this.todo);

        if (msg !== "OK") {
          this.validationMsg = msg;
          return;
        }

        // reset validation message
        this.validationMsg = null;

        // Send updated todo up to parent
        this.$emit("update-todo", this.todo);

        this.edit = false;
      },
      startEditMode() {
        this.edit = true;
      },
      endEditMode() {
        this.edit = false;
      },
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
    },
  };
</script>

<style lang="scss" scoped>
  @import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css");

  .card:hover {
    background-color: darken($color: #fafafa, $amount: 8%);
  }

  .pointer {
    cursor: pointer;
  }

  .todo-info {
    min-width: 150px;
    margin-right: 5px;
  }

  .btn-group {
    margin: auto 0;

    .v-bottom {
      margin-right: 20px;
      vertical-align: -webkit-baseline-middle;
    }
  }

  .todo-item {
    padding: 10px;

    .flex-container {
      display: flex;
      justify-content: space-between;

      input[type="checkbox"] {
        width: 20px;
        height: 100%;
        margin-right: 26px;
        cursor: pointer;
      }
    }

    .deadline-text {
      font-size: 0.8em;
      color: gray;
    }

    .assignee-text {
      padding: 3px 5px;
      margin-left: 4px;
      font-size: 0.6em;
      color: white;
      border-radius: 8px;
      background-color: #27a5d3;
    }

    .pre-text {
      font-size: 0.6em;
      color: gray;
    }
  }

  input[type="text"],
  input[type="date"],
  textarea {
    width: 100%;
    margin: 10px 0px;
    padding: 10px;
    background-color: #f0f0f0 !important;
    border: 1px solid rgb(192, 192, 192);
  }

  input[type="button"],
  input[type="submit"] {
    margin: 10px 0px 10px 20px;
    background-color: #33b5e5;
  }

  .keep-inline {
    display: flex;
    justify-content: flex-end;
  }

  .is-complete {
    p {
      text-decoration: line-through;
    }
  }

  .del {
    color: rgb(48, 48, 48);
    background: none;
    font-size: 24px;
    border: none;
    cursor: pointer;
    vertical-align: -webkit-baseline-middle;

    &:focus {
      outline: 0;
    }
  }

  .expand-enter-active,
  .expand-leave-active {
    overflow: hidden;
    transition: all 0.3s ease-in-out;
  }

  .expand-enter,
  .expand-leave-to {
    height: 0px;
  }

  .expand-leave,
  .expand-enter-to {
    height: 350px;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: all 0.3s ease-in-out;
  }

  .fade-enter,
  .fade-leave-to {
    opacity: 0%;
    max-height: 0px;
    padding: 0px;
  }

  .fade-leave,
  .fade-enter-to {
    opacity: 100%;
    max-height: 100px;
    padding: 10px;
  }

  @media (max-width: 530px) {
    .card {
      padding: 2px 2px;
    }

    .todo-item {
      padding: 4px 4px;

      p,
      small {
        font-size: 2.5vw;
      }
    }

    .todo-info {
      font-size: 2.4vw;
    }

    input[type="checkbox"] {
      margin: 0 14px 0 8px !important;
    }
  }

  @media (max-width: 434px) {
    p,
    small {
      font-size: 0.6em !important;
    }

    .todo-info {
      min-width: 40px;
      font-size: 0.8em;
    }

    input[type="checkbox"] {
      margin: 0 14px 0 8px !important;
    }

    .btn-group {
      i {
        margin-left: 7px !important;
        margin-right: 5px !important;
      }
      button {
        margin-left: 5px;
        margin-right: 5px;
      }
    }
  }
</style>
