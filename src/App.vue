<template>
  <div>

    <nav class="navbar navbar-dark bg-dark">
        <router-link to="/">
<div>
            <h2 class="btn btn-warning stretched-link">Twooter</h2>
</div>
        </router-link>
        <div v-if="user">
            <h3 class="btn btn-primary">{{user.username}}</h3>
        </div>
    </nav>
    <router-view/>
  </div>
</template>
<script>
    import { useStore } from 'vuex';
    import { computed } from 'vue';

    export default {
    name: 'App',
        setup(){
        const store = useStore();
        const user =computed(()=> store.state.User.user);

        return{
            user
        }
        }


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
