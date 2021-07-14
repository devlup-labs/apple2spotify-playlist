<template lang='pug'>
div
  h1 Apple2Spotify-Playlist
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  components: {},
  data() {
    return {
      step: 1,
      code: null,
    };
  },
  methods: {
    stateUpdater() {
      let urlParams = new URLSearchParams(window.location.search);
      if (urlParams.get("code")) {
        this.code = urlParams.get("code");
      }
      const url = new URL("http://localhost:8080/");
      window.history.pushState({}, "", url);
      if (this.code !== null) {
        this.step = 2;
      }
    },
  },
  mounted() {
    this.stateUpdater();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #394f64;
  margin-top: 60px;
}
</style>
