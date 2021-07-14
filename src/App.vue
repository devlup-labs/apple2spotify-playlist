<template lang='pug'>
div
  h1(class="text-4xl m-5") Apple2Spotify-Playlist
  div
   input(class="text border py-2 px-3 text-grey-darkest rounded-xl m-5 border-blue-400" v-model="message" placeholder="Enter the apple playlist link here")
   div(v-for="item in Items")
      label {{item.text}}
      input(type="checkbox" :value="item.text" v-model="checkedItems" :disabled="checkedItems.length>0 && checkedItems.indexOf(item.text) == -1")
  button(class="rounded-lg border-blue-400 border-2 m-5 w-20" @click="proof") Convert
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data(){
    return{
      message:'',
      Items:[
        {id:0 , text: 'private'},
        {id:1 , text: 'public'}
      ],
      checkedItems : []
    }
  },
  methods:{
    proof(){
      console.log(this.message)
      console.log(this.checkedItems)
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
