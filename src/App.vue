<template>
  <div id="app" :style="getMargin()">
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
  },
  methods: {
    getMargin() {
      if (window.innerWidth <= 768) {
        return {'margin-left': this.currentStoryIndex * -100 + 'vw'};
      } 

      return {'margin-top': this.currentStoryIndex * -100 + 'vh'};
    }
  }
}
</script>

<style>
html, body {
  width: 100vw;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}

body {
  font-family: -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
  background: #fafafa;
}

#app {
  transition: margin .3s ease-out;
}

@media (max-width: 768px) {
  html, body {
    height: 100vh;
    overflow: hidden;
  }

  #app {
    width: 9999vw;
    position: relative;    
  }
}
</style>
