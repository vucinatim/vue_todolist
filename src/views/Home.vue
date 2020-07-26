<template>
  <div class="home">
    <div class="center-self create-new-box" @click="createNewList">
      <div class="text-icon">+</div>
      <p class="subtitle">Create a new Todolist</p>
    </div>
    <div class="center-self" v-for="list in lists" v-bind:key="list.id">
      <EnterListBtn v-bind:list="list" />
    </div>
  </div>
</template>

<script>
  import EnterListBtn from "@/components/EnterListBtn.vue";
  import { v4 as uuidv4 } from "uuid";

  export default {
    name: "Home",
    components: {
      EnterListBtn,
    },
    props: ["lists"],
    methods: {
      createNewList(e) {
        e.preventDefault();

        const newList = {
          id: uuidv4(),
          title: "New Todo List",
          todos: [],
        };
        // Send new list up to parent
        this.$emit("create-list", newList);
      },
    },
  };
</script>

<style lang="scss" scoped>
  .home {
    display: grid;
    grid-gap: 40px;
    grid-template-columns: auto auto auto;
    align-items: center;
  }

  .center-self {
    justify-self: center;
  }

  .create-new-box {
    width: 170px;
    height: 170px;
    margin: 10px 0;
    border: 3px dotted rgb(124, 124, 124);
    color: rgb(124, 124, 124);
    position: relative;
    cursor: pointer;

    transition: all 0.5s ease-in-out;

    &:hover {
      color: #27a5d3;
      border-color: #27a5d3;

      .text-icon {
        font-size: 70pt;
      }
    }

    .text-icon {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 60pt;
      font-weight: lighter;
      margin: auto;
      text-align: center;
      transition: font-size 0.2s ease-in-out;
    }

    .subtitle {
      position: absolute;
      bottom: 0;
      margin: 10px 0;
      font-size: 8pt;
      width: 100%;
      text-align: center;
    }
  }
</style>
