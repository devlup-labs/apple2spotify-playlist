<template lang='pug'>
.stepper.pt-48
  h1.text.text-white.text-3xl.pb-10.mb-10.tracking-wide Spotify Login
  p.text.text-white.tracking-wider.pt-10 Login To Spotify for Converting your Apple Playlist
  button(@click="loggingToSpotify" class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-20 pt-2 pb-3 text-black rounded-full bg-white ")
    span.tracking-widest.pr-7.pl-7.font-bold LOG IN
</template>

<script>
export default {
  data(){
    return {
      clientId: "SPOTIFY_CLIENT_ID",
      redirectUri:"http://localhost:8080/", 
      spotifyScopes: "user-read-email playlist-modify-public playlist-modify-private", 
    }
  },
  methods:{
    loggingToSpotify() { 
      var url =`https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.spotifyScopes}&state=34fFs29kd09`;
      window.location.href = url; 
      }, 
  }
}
</script>

<style>
.stepper{
    height: 800px;
}

</style>