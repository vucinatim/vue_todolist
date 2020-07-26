<template>
  <div>
    <Header class="sticky" />
    <div id="app" class="container">
      <router-view
        v-on:create-list="createList"
        v-on:update-list="updateList"
        v-on:remove-list="removeList"
        v-bind:lists="lists"
      />
    </div>
  </div>
</template>

<script>
  import Header from "./components/layout/Header";

  const STORAGE_KEY = "todo-storage";

  export default {
    name: "app",
    components: {
      Header,
    },
    data() {
      return {
        lists: [],
      };
    },
    created() {
      this.lists = JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
    },
    methods: {
      createList(newList) {
        this.lists = [...this.lists, newList];
        localStorage.setItem(STORAGE_KEY, JSON.stringify(this.lists));
      },
      updateList(updatedList) {
        this.lists.forEach((item, i) => {
          if (item.id == updatedList.id) this.lists[i] = updatedList;
        });
        localStorage.setItem(STORAGE_KEY, JSON.stringify(this.lists));
      },
      removeList(id) {
        this.lists = this.lists.filter((list) => list.id != id);
        localStorage.setItem(STORAGE_KEY, JSON.stringify(this.lists));
      },
    },
  };
</script>

<style lang="scss">
  $base-color: #33b5e5;
  $light-gray-color: rgb(216, 216, 216);

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  html,
  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
    margin: 0;
    height: 100%;
    background-image: linear-gradient(#d8d8d8, #b6b6b6);
    background-attachment: fixed;
  }

  .text-center {
    text-align: center;
  }

  .sticky {
    z-index: 100;
    width: 100%;
    position: fixed;
    top: 0;
  }

  .m-auto {
    margin: auto;
  }

  .container {
    max-width: 700px;
    margin: 0 auto;
    padding: 60px 20px;
  }

  .flex {
    display: flex;
  }

  .content-between {
    justify-content: space-between;
  }

  .card {
    background-color: #f0f0f0;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
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

  .validation-msg {
    text-align: center;
    color: #33b5e5;
    margin: 10px 0px;
  }

  /* Solid border */
  hr.solid {
    margin: 8px 0px;
    border: none;
    border-bottom: 1px solid rgb(139, 139, 139);
  }

  .float-right {
    float: right;
  }

  .float-left {
    float: left;
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

  @media (max-width: 630px) {
    .home {
      grid-template-columns: auto auto !important;
    }

    .list-box {
      p {
        font-size: 2.6vw !important;
      }
    }
  }

  @media (max-width: 434px) {
    .home {
      grid-template-columns: auto !important;
    }

    .list-box {
      p {
        font-size: 0.9em !important;
      }
    }
  }

  @media (min-width: 850px) {
    .list-box {
      p {
        font-size: 1em !important;
      }
    }
  }
</style>
