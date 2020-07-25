<template>
    <div>
        <Header />
        <div id="app" class="container">
            <router-view/>
        </div>
    </div>
</template>

<script>
import Header from './components/layout/Header';

export default {
  name: "app",
  components: {
    Header
  }
}
</script>

<style lang="scss">

    $base-color: #33b5e5;
    $light-gray-color: rgb(216, 216, 216);

    * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    }

    html, body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
    margin: 0;
    height: 100%;
    background-image: linear-gradient(#d8d8d8, #b6b6b6);
    }

    .container {
        max-width: 600px;
        margin: 0 auto;
    }

    .flex {
        display: flex;
    }

    .content-between {
        justify-content: space-between;
    }

    .card {
    background-color: #f0f0f0;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    margin: 10px 0px;
    padding: 4px 16px;
    }

    .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #f0f0f0;
    padding: 7px 20px;
    cursor: pointer;
    }

    .btn:hover {
    background: #666;
    }

    .keep-inline {
        display: inline;
        float: right;
    }

    #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    /* text-align: center; */
    color: #2c3e50;
    }

    #nav {
    padding: 30px;
    }

    #nav a {
    font-weight: bold;
    color: #2c3e50;
    }

    #nav a.router-link-exact-active {
    color: #42b983;
    }
</style>
