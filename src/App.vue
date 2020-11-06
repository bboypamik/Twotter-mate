<template>
  <div>

    <nav class="navbar navbar-dark bg-dark">
        <router-link to="/">
<h2 class="btn btn-warning stretched-link">Twooter</h2>
        </router-link>
    </nav>
    <router-view/>
  </div>
</template>
<script>

    export default {
    name: 'App',

  }
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

h2{
  color: white;
}

#nav {
  padding: 30px;


  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
