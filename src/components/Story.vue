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
import Hammer from 'hammerjs';
import { EventBus } from '../helpers/EventBus.js';

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
      this.resetSlide();
    },
    deactivate: function() {
      this.timeline.pause();
    },
    resetSlide: function() { // Jump to beginning of the slide
      this.timeline.pause();
      this.timeline.seek(this.currentSlideIndex * SLIDE_DURATION);
      this.timeline.play();
    },
    nextSlide: function() {
      if (this.currentSlideIndex < this.slides.length - 1) {
        this.currentSlideIndex++;
        this.resetSlide();
      } else {
        this.nextStory();
      }
    },
    previousSlide: function() {
      if (this.currentSlideIndex > 0) {
        this.currentSlideIndex--;
        this.resetSlide();
      } else {
        this.previousStory();
      }
    }, 
    nextStory: function() {
      EventBus.$emit('NEXT_STORY');
    },
    previousStory: function() {
      EventBus.$emit('PREVIOUS_STORY');
    }
  },
  mounted() {   
    let $timeline = this.$el.getElementsByClassName('timeline')[0];

    // Add progress bars to the timeline animation group
    this.slides.forEach((color, index) => {  
      this.timeline.add({
        targets: $timeline.getElementsByClassName('slice')[index].getElementsByClassName('progress'),
        width: '100%',
        changeBegin: () => {
          // Update the Vue componenet state when progress bar begins to play
          this.currentSlideIndex = index;
        },
        complete: (anim) => {
          // Move to the next story when finished playing all slides
          if (index === this.slides.length - 1) {
            this.nextStory();
          }
        }
      });
    });

    this.hammer = new Hammer.Manager(this.$el, {
      recognizers: [
        [Hammer.Pan, { direction: Hammer.DIRECTION_HORIZONTAL }],
        [Hammer.Tap],
        [Hammer.Press, { time: 1, threshold: 1000000 }]
      ]
    })

    this.hammer.on("press", () => {
      this.timeline.pause();
    });

    this.hammer.on("pressup tap", () => {
      this.timeline.play();
    });

    // Tap on the side to navigate between slides
    this.hammer.on("tap", event => {
      if (event.center.x > window.innerWidth / 3) {
        this.nextSlide();
      } else {
        this.previousSlide();
      }
    });

    // Handle swipe
    this.hammer.on("pan", event => {
      if (event.isFinal) {
        if (event.deltaX < 0) {
          this.nextStory();
        } else if (event.deltaX > 0) {
          this.previousStory();
        }
      }
    });
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
