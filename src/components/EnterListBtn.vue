<template>
  <router-link :to="{ name: 'TodoList', params: { id: list.id } }">
    <div class="list-box card">
      <p class="text-center responsive-size">{{ list.title }}</p>
      <hr class="solid" />
      <div class="fade-bottom">
        <div
          v-for="todo in list.todos"
          v-bind:key="todo.id"
          class="flex-center"
        >
          <i
            v-if="todo.completed"
            class="float-left fa fa-check-square-o"
            aria-hidden="true"
          ></i>
          <i
            v-if="!todo.completed"
            class="float-left fa fa-square-o"
            aria-hidden="true"
          ></i>
          <p v-if="todo.title.length <= 15" class="smallTodoTitle">
            {{ todo.title }}
          </p>
          <p v-else class="smallTodoTitle">
            {{ todo.title.substring(0, 15) + ".." }}
          </p>
        </div>
      </div>
    </div>
  </router-link>
</template>

<script>
  export default {
    name: "EnterListBtn",
    props: ["list"],
  };
</script>

<style lang="scss" scoped>
  a {
    text-decoration: none;
    color: gray;
  }
  .card,
  hr {
    transition: all 0.5s ease-in-out;
    &:hover {
      color: #27a5d3;
      hr {
        border-color: #27a5d3;
      }
    }
  }

  .responsive-size {
    font-size: 2vw;
  }

  .flex-center {
    display: flex;
    align-items: center;
  }

  .smallTodoTitle {
    font-size: 0.9em;
    margin-left: 5px;
  }

  .fade-bottom {
    height: 100%;
    max-height: 115px;
    -webkit-mask-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 1) 70%,
      rgba(0, 0, 0, 0) 90%
    );
    -webkit-mask-size: 100% 100%;
    -webkit-mask-repeat: no-repeat;
    -webkit-mask-position: bottom;
  }

  .list-box {
    height: 170px;
    width: 170px;
    padding: 10px 16px;
    position: relative;
    overflow: hidden;
    background-color: #dbdbdb;
    cursor: pointer;

    &:hover:after {
      animation: shine 5s ease-in-out infinite;
      animation-fill-mode: forwards;
      content: "";
      position: absolute;
      top: -110%;
      left: -210%;
      width: 200%;
      height: 200%;
      opacity: 0;
      transform: rotate(30deg);

      background: rgba(255, 255, 255, 0.13);
      background: linear-gradient(
        to right,
        rgba(255, 255, 255, 0.13) 0%,
        rgba(255, 255, 255, 0.13) 77%,
        rgba(255, 255, 255, 0.5) 92%,
        rgba(255, 255, 255, 0) 100%
      );
    }

    &:active:after {
      opacity: 0;
    }
  }

  @keyframes shine {
    10% {
      opacity: 1;
      top: -30%;
      left: -30%;
      transition-property: left, top, opacity;
      transition-duration: 0.7s, 0.7s, 0.15s;
      transition-timing-function: ease;
    }
    100% {
      opacity: 0;
      top: -30%;
      left: -30%;
      transition-property: left, top, opacity;
    }
  }
</style>
