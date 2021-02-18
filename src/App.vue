<template>
  <Weather />
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import Weather from './components/Weather.vue'

export default {
  name: 'App',
  components: {
    Weather,
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
  margin-top: 60px;
}
html, body {
  height: 100%;
  background-color: lightblue;
  padding-top: 5rem;
}
</style>
