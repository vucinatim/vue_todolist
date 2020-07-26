<template>
  <router-link :to="{ name: 'TodoList', params: { list: list } }">
    <div class="list-box card">
      <p>{{ list.id }}</p>
      <p v-for="todo in list.todos" v-bind:key="todo.id">{{ todo.title }}</p>
    </div>
  </router-link>
</template>

<script>
  export default {
    name: "EnterListBtn",
    props: {
      list: {},
    },
  };
</script>

<style lang="scss" scoped>
  a {
    text-decoration: none;
    color: gray;
  }

  .list-box {
    height: 170px;
    width: 170px;
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
