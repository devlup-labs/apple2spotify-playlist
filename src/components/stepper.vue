<template lang='pug'>
.stepper.mx-12.text-left
  div.text-4xl.tracking-wider(v-bind:class = "(step === 1)?'text-white font-bold':'text-green-500 font-semibold'") Step-1
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 1")
    h1.text-white.text-3xl.pb-2 Spotify Login
    p.text-white.tracking-wider.pt-2 Login To Spotify for Converting your Apple Playlist
    button(@click="loggingToSpotify" class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white ")
      span.tracking-widest.px-7.font-bold LOG IN
    button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white" @click="addStep")
      span.tracking-widest.px-7 Next Step
  br
  div.text-4xl.tracking-wider(v-bind:class = "(step === 2)?'text-white font-bold':(step < 2)?'text-gray-600':'text-green-500 font-semibold'") Step-2
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 2")
    button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white" @click="addStep")
      span.tracking-widest.px-7 Convert
  br
  div.text-4xl.tracking-wider(v-bind:class = "(step === 3)?'text-white font-bold':(step < 3)?'text-gray-600':'text-green-500 font-semibold'") Step-3
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 3")
    button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white" @click="addStep")
      span.tracking-widest.px-7 Done
  
</template>

<script>
export default {
  data(){
    return {
      clientId: "SPOTIFY_CLIENT_ID",
      redirectUri:"http://localhost:8080/", 
      spotifyScopes: "user-read-email playlist-modify-public playlist-modify-private",
      step: 1, 
    }
  },
  methods:{
    loggingToSpotify() { 
      var url =`https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.sotifyScopes}&state=34fFs29kd09`;
      window.location.href = url; 
      },
    addStep() {
      this.step += 1;
    },
    changeColor() {

    } 
  }
}
</script>

<style>
.stepper{
    height: 800px;
}
.box{
  margin-left: 20px;
  margin-right: 20px;
  padding: 30px;
}

</style>