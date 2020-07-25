<template>
    <div class="card">
        <div class="todo-item" v-bind:class="{'is-complete':todo.completed}">
          <div class="flex-container">
              <div class="flex-item">
                  <input type="checkbox" v-model="todo.completed">
                  <div class="keep-inline">
                      <p>{{ todo.title }}</p>
                      <small>{{ todo.description }}</small>
                  </div>
              </div>
              <small>{{ timeUntilDeadline(todo.deadline) }}</small>
              <button @click="$emit('del-todo', todo.id)" class="flex-item del"> &times; </button>
          </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "TodoItem",
    props: ["todo"],
    methods: {
      timeUntilDeadline(deadline) {
        var diffTime = deadline - new Date();
        var diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        if (diffDays > 0)
          return diffDays + " days left.";
        else
          return "Deadline reached!";
      }
    }
}
</script>

<style lang="scss" scoped>

    .card:hover {
      background-color: darken($color: #fafafa, $amount: 5%);
    }

    .todo-item {
        padding: 10px;

        &:hover {
          cursor: pointer;
        }

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
</style>