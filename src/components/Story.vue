<template>
  <div class="story">
    <div class="timeline">
      <div class="slice">
        <div class="progress">&nbsp;</div>
      </div>
    </div>
    <div class="slide">
      <p>Hello World</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Story',
  props: {
    msg: String
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
