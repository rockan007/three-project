<template>
  <div id="app" class="container d-flex flex-column">
    <div  id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view class="flex-grow-1 flex-shrink-1"/>
  </div>
</template>
<script>
import "bootstrap/dist/css/bootstrap.min.css";
export default {
  name: "app"
};
</script>

<style>
html,
body,
.container {
  width: 100%;
  height: 100%;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
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
