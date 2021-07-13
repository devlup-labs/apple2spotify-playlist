<template lang='pug'>
div
  h1 Apple2Spotify-Playlist
Home
</template>

<script>
import axios from "axios";
import Home from "./components/Home.vue"
export default {
  name: 'App',
  components: {
    Home,
  }
}

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
