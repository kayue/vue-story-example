<template>
  <div id="app" :style="{'margin-left': currentStoryIndex * -100 + 'vw'}">
    <Story :slides="story" v-for="(story, index) in stories" :key="index" ref="stories" /> 
  </div>
</template>

<script>
import Story from './components/Story.vue';
import { EventBus } from './helpers/EventBus.js';

export default {
  name: 'app',
  components: {
    Story
  },
  data() {
    return {
      currentStoryIndex: 0,
      stories: [
        ["#D53738", "#638867"],
        ["#DAF7A6", "#FFC300", "#FF5733"],
        ["#00BCD4"]
      ]
    };
  },
  mounted () {
    EventBus.$on('NEXT_STORY', () => {
      if (this.currentStoryIndex < this.stories.length - 1) {
        this.$refs.stories[this.currentStoryIndex].deactivate();
        this.currentStoryIndex++;
        this.$refs.stories[this.currentStoryIndex].activate();
      }
    });
    
    EventBus.$on('PREVIOUS_STORY', () => {
      if (this.currentStoryIndex > 0) {
        this.$refs.stories[this.currentStoryIndex].deactivate();
        this.currentStoryIndex--;
        this.$refs.stories[this.currentStoryIndex].activate();
      } else {
        this.$refs.stories[this.currentStoryIndex].resetSlide();
      }
    });

    this.$refs.stories[0].activate();
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
