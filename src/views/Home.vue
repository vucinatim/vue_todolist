<template>
  <div class="home">
    <div v-for="list in todoLists" v-bind:key="list.id">
      <EnterListBtn />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import EnterListBtn from '@/components/EnterListBtn.vue'

export default {
  name: 'Home',
  components: {
    EnterListBtn
  },
  props: ["todoLists"]
}
</script>

<style lang="scss" scoped>

</style>