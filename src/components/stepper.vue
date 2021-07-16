<template lang='pug'>
.stepper.pt-48
  h1.text.text-white.text-3xl.pb-10.mb-10.tracking-wide Spotify Login
  p.text.text-white.tracking-wider.pt-10 Login To Spotify for Converting your Apple Playlist
  button(@click="loggingToSpotify" class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-20 pt-2 pb-3 text-black rounded-full bg-white ")
    span.tracking-widest.pr-7.pl-7.font-bold LOG IN
  div(class="field")
   h1.text-white.text-3xl.mt-10.mb-2.tracking-wide Playlist link
   input(class="text-gray-700 font-bold mb-2 rounded-full w-72 h-7" type="text" v-model="plink" placeholder="Enter the apple playlist link here")
  div(class="mt-5")
   label.text-white.tracking-wider.pt-10 Make your playlist private
   input(class="btn ml-2 h-4 w-4" type="checkbox" v-model="isprivate")
  button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-10 pt-2 pb-3 text-black rounded-full bg-white")
   span.tracking-widest.pr-7.pl-7.font-bold CONVERT
</template>

<script>
export default {
  data(){
    return {
      clientId: "SPOTIFY_CLIENT_ID",
      redirectUri:"http://localhost:8080/", 
      spotifyScopes: "user-read-email playlist-modify-public playlist-modify-private",
      plink:'',
      isprivate : false,
      spotifyScopes: "user-read-email playlist-modify-public playlist-modify-private", 
      isprivate:false,
      plink:''
    }
  },
  methods:{
    loggingToSpotify() { 
      var url =`https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.sotifyScopes}&state=34fFs29kd09`;
      window.location.href = url; 
      }
  }
}
</script>

<style>
.stepper{
    height: 800px;
}

.field{
  position:relative;
}
.btn{
  position:relative;
}

</style>