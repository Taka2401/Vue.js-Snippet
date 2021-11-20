<template>
  <div id="app">
    <Index />
  </div>
</template>

<script>
// import Top from './components/Top.vue'
import Index from './components/Index.vue'

export default {
  name: 'App',
  components: {
    // Top,
    Index
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
</style>
