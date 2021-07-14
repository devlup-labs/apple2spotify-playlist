<template lang='pug'>
div
  h1(class="text-4xl m-5") Apple2Spotify-Playlist
  div
   input(class="text border py-2 px-3 text-grey-darkest rounded-xl m-5 border-blue-400" v-model="message" placeholder="Enter the apple playlist link here")
   div
      label Make your playlist private
      input(class="ml-2 h-4 w-4" type="checkbox" v-model="private")
  button(class="rounded-lg border-blue-400 border-2 m-5 w-20") Convert
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      message:'',
      private : false
    }
  }
}
</script>

<style>
#app{
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #394f64;
  margin-top: 60px;
}
</style>
