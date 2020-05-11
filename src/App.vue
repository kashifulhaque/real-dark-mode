<template>
  <div id="app">
    <div class="overlay" @mousemove="shineLight"></div>

    <h1>Real dark mode and r/TwoSentenceHorror</h1>

    <br /><br /><br />

    <div
      class="story"
      v-for="(horrorStory, i) in horrorStories"
      :key="horrorStory.data.id"
    >
      <p>
        {{ horrorStory.data.title }}
      </p>

      <p>
        {{ horrorStory.data.selftext }}
      </p>

      <br />
      <br />
    </div>
  </div>
</template>

<script>
// Based on a Glitch project which I found on Reddit. Link: https://glitch.com/~paranoids-flashlight
// Imports
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      baseUrl: "https://www.reddit.com/r/TwoSentenceHorror.json?limit=25",
      overlay: "",
      horrorStories: [],
    };
  },
  methods: {
    shineLight(e) {
      this.overlay.style.background = `radial-gradient(circle at ${e.pageX}px ${e.pageY}px, transparent 0%, black 8em`;
    },
  },
  mounted() {
    this.overlay = document.querySelector(".overlay");
  },
  created() {
    axios
      .get(this.baseUrl)
      .then((res) => {
        this.horrorStories = res.data.data.children;
        this.horrorStories.shift(); // To remove the first pinned post from r/TwoSentenceHorror, this might break in the future
      })
      .catch((err) => {
        console.log(err);
      });
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Major+Mono+Display&display=swap");

* {
  margin: 0;
  padding: 0;
  background-color: #131313;
}

html {
  min-height: 100%;
  position: relative;
}

body {
  background: #f3f3f3;
  height: 100%;
  max-width: 640px;
  margin: 2rem auto;
}

#app {
  color: #f3f3f3;
}

h1 {
  font-family: "Major Mono Display", monospace;
}

p {
  font-family: "Major Mono Display", monospace;
}

.overlay {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  z-index: 100;
  background: #1b1b1b;
  overflow: hidden;
  cursor: url("https://image.flaticon.com/icons/svg/107/107715.svg"), none;
}
</style>
