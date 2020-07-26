<template>
  <header class="header">
    <h5 class="header-text">Tim Vucina</h5>
    <router-link class="header-text" to="/">Home</router-link>
  </header>
</template>

<script>
  export default {
    name: "Header",
  };
</script>

<style lang="scss" scoped>
  .header {
    background: linear-gradient(60deg, #7cd1f0, #33b5e5);
    color: #fff;
    text-align: center;
    padding: 10px;
    display: flex;
    justify-content: space-between;

    a {
      color: #fff;
      padding-right: 5px;
      text-decoration: none;
    }

    .header-text {
      font-size: 1em !important;
      font-weight: normal !important;
      font-family: Arial, Helvetica, sans-serif !important;
    }
  }
</style>
