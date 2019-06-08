<template>
  <div id="app">
    <Story :slides="story" v-for="(story, index) in stories" :key="index" /> 
  </div>
</template>

<script>
import Story from './components/Story.vue'

export default {
  name: 'app',
  components: {
    Story
  },
  data() {
    return {
      stories: [
        ["#D53738", "#638867"],
        ["#DAF7A6", "#FFC300", "#FF5733"],
        ["#00BCD4"]
      ]
    };
  },
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
