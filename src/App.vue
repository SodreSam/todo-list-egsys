<template>
  <div id="container">
    <img class="logo" src="./assets/boltlogo.png">
    <todo-list/>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>

* {
  box-sizing: border-box;
}

.container { 
  max-width: 100px;
  margin: 0 auto;
  margin-top: 200px;
}


#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 200px;
}

.logo {
  display: block;
  height: 80px;
  margin: 20px auto;
  margin-top: 120px;
}
</style>
