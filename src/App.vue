<template>
  <header class="navbar-nav navbar-expand-sm bg-light">
    <nav class="mx-auto p-4">
      <ul class="navbar-nav mx-auto" style="width:max-content">
        <li class="navbar-item px-3"><router-link to="/">Home</router-link></li>
        <li class="navbar-item px-3"><router-link to="/tools" :class="toolsNav">Tools</router-link></li>
        <li class="navbar-item px-3"><router-link to="/about">About</router-link></li>
      </ul>
    </nav>
  </header>
  <!-- <p style="background:linear-gradient(to left, #de66f8, #ff6666);height:2px;"></p> -->
  <router-view/>
</template>

<script>
  export default {
    name: "App",
    data() {
      return {
        toolsNav: ""
      }
    },
    watch: {
      $route(to, from){
        if (to.path.includes('/tools')){
          this.toolsNav = "router-link-exact-active";
        }
        else {
          this.toolsNav = ""
        }
      }
    }
  }
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #f1592e;
}
[v-cloak] {
  display: none;
}
.relative {
  position: relative;
}
.absolute {
  position: absolute;
}
.fixed {
  position: fixed;
}
</style>
