<template>
  <div class="story" :style="{ background: slides[currentSlideIndex] }">
    <div class="timeline">
      <div class="slice" v-for="(slide, i) in slides" :key="i">
        <div class="progress">&nbsp;</div>
      </div>
    </div>
    <div class="slide">
      <p>{{ slides[currentSlideIndex] }}</p>
    </div>
  </div>
</template>

<script>
import anime from 'animejs/lib/anime.es.js';

const SLIDE_DURATION = 2000;

export default {
  name: 'Story',
  props: {
    slides: Array
  },
  data() {
    const timeline = anime.timeline({
      autoplay: false,
      duration: SLIDE_DURATION,
      easing: 'linear'
    });

    return {
      currentSlideIndex: 0,
      isActive: false,
      timeline: timeline,
    }
  },
  methods: {
    activate: function() { // Start timer
      this.timeline.play();
    },
    deactivate: function() {
      this.timeline.pause();
    },
    nextSlide: function() {}, // Tap right to skip to next slide
    previousSlide: function() {}, 
    nextStory: function() {}, // Swipe, or reach the last slide
    previousStory: function() {}, 
  },
  mounted() {   
    let $timeline = this.$el.getElementsByClassName('timeline')[0];

    // Add progress bars to the timeline animation group
    this.slides.forEach((color, index) => {  
      this.timeline.add({
        targets: $timeline.getElementsByClassName('slice')[index].getElementsByClassName('progress'),
        width: '100%',
        changeBegin: (anim) => {
          // Update the Vue componenet state when progress bar begins to play
          this.currentSlideIndex = index;
        }
      });
    });

    // Play the story
    this.activate();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.story {
  float: left;
  position: relative;
  height: 100vh;
  width: 100vw;
  z-index: 1;
  display: flex;
  flex-direction: column;
}

.timeline {
  display: flex;
  flex-grow: 0;
  width: 100%;
}

.timeline > .slice {
  background: rgba(0,0,0,0.25);
  height: 10px;
  margin: 10px;
  width: 100%;
}

.timeline > .slice > .progress {
  background: black;
  height: 10px;
  width: 0%;
}

.slide {
  /* Take the rest of the page */
  flex-grow: 1; 

  /* Center align */
  display: flex;
  align-items: center;
  justify-content: center;
}

.slide p {
  font-size: 60px;
  opacity: .5;
}
</style>
