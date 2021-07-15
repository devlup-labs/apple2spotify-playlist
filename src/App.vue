<template lang="pug">
#app
  .text.mx-auto
    nav.p-12.text-center.font-semibold.text-5xl.text-white(class="md:text-7xl")
      h1.tracking-in-contract-bck Playlistify
    Home(@clicked="goto('stepper')")
    .my-80(ref="stepper")
      Stepper
  Particles#tsparticles(:options="options")
</template>

<script>
import axios from "axios";
import Home from "./components/home.vue";
import Stepper from "./components/stepper.vue";
import options from "./particles.json";
import qs from 'qs';
export default {
  name: 'App',
  components: { Home, Stepper },
  data() {
    return {
      step: 1,
      CODE: null,
      token: null,
        headers: {
            headers:{
              'Content-type':'application/x-www-form-urlencoded'
            }
        },
      options: options
    };
  },
  methods: {
    goto(refName) {
      var element = this.$refs[refName];
      var top = element.offsetTop;
      window.scroll({ left: 0, top, behavior: "smooth" });
    },
    code() {

      var DATA = {
            grant_type:"authorization_code",
            code:this.CODE,
            redirect_uri:"http://localhost:8080/",
            client_id:"SPOTIFY_CLIENT_ID",
            client_secret:"SPOTIFY_CLIENT_SECRET"
        };

      var data = qs.stringify(DATA);
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
    },
    updateStep() {
      let urlParams = new URLSearchParams(location.search);
      if (urlParams.get("code")) {
        this.CODE = urlParams.get("code");
        const url = [location.protocol, '//', location.host, location.pathname].join('');
        window.history.pushState({}, "", url);
      }
      if (this.CODE !== null) {
        this.step = 2;
        this.code();
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
nav {
  font-family: "Noto Sans JP", sans-serif;
}
.tracking-in-contract-bck {
  -webkit-animation: tracking-in-contract-bck 2s
    cubic-bezier(0.215, 0.61, 0.355, 1) both;
  animation: tracking-in-contract-bck 2s cubic-bezier(0.215, 0.61, 0.355, 1)
    both;
}
@-webkit-keyframes tracking-in-contract-bck {
  0% {
    letter-spacing: 1em;
    -webkit-transform: translateZ(400px);
    transform: translateZ(400px);
    opacity: 0;
  }
  40% {
    opacity: 0.6;
  }
  100% {
    -webkit-transform: translateZ(0);
    transform: translateZ(0);
    opacity: 1;
  }
}
.tsparticles {
  position: absolute;
}
.text {
  position: absolute;
  width: 100%;
  height: 100%;
}
</style>