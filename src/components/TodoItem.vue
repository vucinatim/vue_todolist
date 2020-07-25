<template>
    <div class="card">

        <transition name="fade">
          <div class="todo-item" v-if="!edit" v-bind:class="{'is-complete':todo.completed}">
            <div class="flex-container">
                <div class="flex-item">
                    <input type="checkbox" v-model="todo.completed">
                    <div class="keep-inline">
                        <p>{{ todo.title }}</p>
                        <small>{{ todo.description }}</small>
                    </div>
                </div>
                <small class="m-auto">{{ timeUntilDeadline(todo.deadline) }}</small>
                <div class="btn-group">
                  <i @click="startEditMode" class="pointer v-bottom fa fa-pencil-square-o" aria-hidden="true"></i>
                  <button @click="$emit('del-todo', todo.id)" class="flex-item del"> &times; </button>
                </div>
            </div>
          </div>
        </transition>

        <transition name="expand">
          <div class="todo-item-edit" v-if="edit">
            <form @submit="updateTodo">
                  <input type="text" v-model="todoTitle" name="todoTitle">
                  <textarea v-model="todoDescription" name="todoDescription"></textarea>
                  <label for="todoDeadline">Deadline: </label>
                  <input type="date" v-model="todoDeadline" name="todoDeadline">
                  <div class="float-right">
                    <input type="submit" value="Save" class="btn">
                    <input type="button" @click="endEditMode" value="Cancel" class="btn">
                  </div>
              </form>
          </div>
        </transition>
    </div>
</template>

<script>
export default {
    name: "TodoItem",
    props: ["todo"],
    data() {
      return {
        edit: false,
        todoTitle: this.todo.title,
        todoDescription: this.todo.description,
        todoDeadline: this.todo.deadline.toISOString().split('T')[0]
      }
    },
    methods: {
      timeUntilDeadline(deadline) {
        var diffTime = deadline - new Date();
        var diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        if (diffDays > 0)
          return diffDays + " days left.";
        else
          return "Deadline reached!";
      },
      updateTodo(e) {
        e.preventDefault();

        this.todo.title = this.todoTitle;
        this.todo.description = this.todoDescription;
        this.todo.deadline = new Date(this.todoDeadline);
        this.edit = false;
      },
      startEditMode() {
        this.edit = true;
      },
      endEditMode() {
        this.edit = false;
      }
    }
}
</script>

<style lang="scss" scoped>

    @import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css");

    .card:hover {
      background-color: darken($color: #fafafa, $amount: 8%);
    }

    .m-auto {
      margin: auto;
    }

    .pointer {
      cursor: pointer;
    }

    .btn-group {
      margin: auto 0;

      .v-bottom {
        margin-right: 20px;
        vertical-align: bottom;
      }
    }

    .todo-item {
        padding: 10px;

        .flex-container {
          display: flex;
          justify-content: space-between;

          input[type="checkbox"] {
            width:20px;
            height:100%;
            margin-right: 26px;
            cursor: pointer;
          }
        }
    }

    input[type="text"], input[type="date"], textarea {
      width: 100%;
      margin: 10px 0px;
      padding: 10px;
      background-color: #f0f0f0 !important;
      border: 1px solid rgb(192, 192, 192);
    }

    input[type="button"], input[type="submit"] {
      margin: 10px 0px 10px 20px;
      background-color: #33b5e5;
    }

    .float-right {
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
        float: right;

        &:focus {
            outline: 0;
        }
    }

    .expand-enter-active, .expand-leave-active {
        overflow: hidden;
        transition: all .3s ease-in-out;
    }

    .expand-enter, .expand-leave-to {
        height: 0px;
    }

    .expand-leave, .expand-enter-to {
        height: 270px;
    }

    .fade-enter-active, .fade-leave-active {
        transition: all .3s ease-in-out;
    }

    .fade-enter, .fade-leave-to {
        opacity: 0%;
        max-height: 0px;
        padding: 0px;

    }

    .fade-leave, .fade-enter-to {
        opacity: 100%;
        max-height: 100px;
        padding: 10px;
    }
</style>