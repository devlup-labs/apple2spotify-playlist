<template lang='pug'>
div
  h1 Apple2Spotify-Playlist

div
  button(type='button', @click.native="code()" ) PressMe
</template>

<script>
import axios from "axios";
import qs from 'qs';


export default {
  name:'App', 
  components:{

  },
  data(){
    return{
      token:"",
      code:CODE,
      data : {
          grant_type:"authorization_code",
          code:CODE,
          redirect_uri:"http://localhost:8080/",
          client_id:"SPOTIFY_CLIENT_ID",
          client_secret:"SPOTIFY_CLIENT_SECRET"
    },
    headers : {
        headers:{
          'Content-type':'application/x-www-form-urlencoded'
        }
      }    
    };
  },
  methods:{
      code: function(){
  
      var data = qs.stringify(this.data);
      var headers = this.headers;
      var URL= 'https://cors.darpan.online/https://accounts.spotify.com/api/token';
      axios({
        method:'post',
        url:URL,
        data,
        headers
      }).then(res =>{

        this.token = res.data.access_token;
        var head = {
          headers:{
            "Accept": "application/json",
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`
          }
        }
        
        axios.get('https://api.spotify.com/v1/me',
        head).then(res =>{
          console.log(res.data);

        }).catch(err =>{
          console.log(err);
        })   
      }).catch(err =>{
        console.log(err);
      })
    }
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
