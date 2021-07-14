<template lang="pug">
h1.text.text-white.text-3xl.pb-5.tracking-wide Spotify Login
p.text.text-white.tracking-wider Login To Spotify for Converting your Apple Playlist
button(@click="spotifyLogin" class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white ")
  span.tracking-widest.pr-7.pl-7.font-bold LOG IN

</template>

<script>
export default {
  data() {
    return {
      clientId: "SPOTIFY_CLIENT_ID",
      redirectUri: "http://localhost:8080/",
      scopes: "user-read-email playlist-modify-public playlist-modify-private",
    };
  },
  methods: {
    spotifyLogin() {
      //var url = `https://cors-anywhere.herokuapp.com/https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.scopes}`
      var url = `https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.scopes}&state=34fFs29kd09`;
      window.location.href = url;
    },
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
