<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>Kinvey HTML5 SDK playground</h1>
    <h3>Testing auth session's duration</h3>
    <HelloWorld msg="Please follow browser's console logs"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
