<template lang="pug">
h1.text.text-white.text-3xl.pb-5.tracking-wide Spotify Login
p.text.text-white.tracking-wider Login To Spotify for Converting your Apple Playlist
button(@click="loggingToSpotify" class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white ")
  span.tracking-widest.pr-7.pl-7.font-bold LOG IN

</template>

<script>
import axios from "axios";
export default {
  name: 'App',
  data() {
    return {
      step: 1,
      code: null,
      clientId: "SPOTIFY_CLIENT_ID",
      redirectUri: "http://localhost:8080/",
      spotifyScopes: "user-read-email playlist-modify-public playlist-modify-private",
    };
  },
  methods: {
    loggingToSpotify() {
      var url = `https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.sotifyScopes}&state=34fFs29kd09`;
      window.location.href = url;
    },
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
  color: #2c3e50;
  margin-top: 60px;
}
.text {
  font-family: "Roboto", sans-serif;
}
.button {
  color: #181818;
  font-family: "Archivo Narrow", sans-serif;
}
</style>
