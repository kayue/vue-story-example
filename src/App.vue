<template>
  <div id="app">
    <Story msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Story from './components/Story.vue'

export default {
  name: 'app',
  components: {
    Story
  }
}
</script>

<style>
html, body {
  height: 100vh;
  width: 100vw;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

body {
  font-family: -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
  background: #fafafa;
}

#app {
  width: 9999vw;
  position: relative;
  transition: margin-left .3s ease-out;
}
</style>
