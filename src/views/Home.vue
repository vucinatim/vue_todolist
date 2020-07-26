<template>
  <div class="home">
    <div class="create-new-box">
      <div class="text-icon">+</div>
      <p class="subtitle">Create a new Todolist</p>
    </div>
    <div v-for="list in lists" v-bind:key="list.id">
      <EnterListBtn v-bind:list="list" />
    </div>
  </div>
</template>

<script>
  import EnterListBtn from "@/components/EnterListBtn.vue";

  export default {
    name: "Home",
    components: {
      EnterListBtn,
    },
    props: ["lists"],
  };
</script>

<style lang="scss" scoped>
  .home {
    display: grid;
    grid-gap: 40px;
    grid-template-columns: auto auto auto;
  }

  .create-new-box {
    width: 170px;
    height: 170px;
    margin: 10px 0;
    border: 3px dotted rgb(124, 124, 124);
    color: rgb(124, 124, 124);
    position: relative;
    cursor: pointer;

    &:hover .text-icon {
      font-size: 70pt;
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
