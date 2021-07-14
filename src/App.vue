<template lang='pug'>
div
  h1 Apple2Spotify-Playlist
</template>

<script>
import axios from "axios";
export default {
  name: 'App',
  data() {
    return {
      step: 1,
      code: null,
    };
  },
  methods: {
    updateStep() {
      let urlParams = new URLSearchParams(location.search);
      if (urlParams.get("code")) {
        this.code = urlParams.get("code");
        const url = [location.protocol, '//', location.host, location.pathname].join('');
        window.history.pushState({}, "", url);
      }
      if (this.code !== null) {
        this.step = 2;
      }
    },
  },
  mounted() {
    this.updateStep();
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
