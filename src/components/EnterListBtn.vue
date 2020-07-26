<template>
  <div class="list-box">
      <p>{{ listId }}</p>
  </div>
</template>

<script>
export default {
  name: 'EnterListBtn',
  props: {
    listId: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

    .list-box {
        height: 100px;
        width: 100px;
        background-color: white;
    }

</style>
