<template>
    <header class="header">
        <h5>Tim Vucina</h5>
        <h5>Todo List</h5>
        <router-link to="/">Home</router-link>
    </header>
</template>

<script>
export default {
    name: "Header"
}
</script>

<style scoped>
    .header {
        background: #33b5e5;
        color: #fff;
        text-align: center;
        padding: 10px;
        display: flex;
        justify-content: space-between;
    }

    .header a {
        color: #fff;
        padding-right: 5px;
        text-decoration: none;
    }
</style>