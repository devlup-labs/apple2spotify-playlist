<template lang="pug">
#app
  .text.mx-auto
    nav.p-12.text-center.font-semibold.text-5xl.text-white(class="md:text-7xl")
      h1.tracking-in-contract-bck Playlistify
    Home(@clicked="goto('stepper')")
    .my-80(ref="stepper")
      div.bg-green-600.rounded-2xl.h-10.w-48.mx-auto(v-show="display" v-bind:class = "(delay === true)?'bounce-in-top':'bounce-out-top'")
        img.py-2.ml-3(src="../public/profile-tick.svg", alt="tick-mark", style="position: absolute")
        h1.text-white.ml-4.py-2.font-bold Login Successful
      Stepper(:step = "step" @addStep="addStep" @gotoStep2="gotoStep2" :spotifyToken="token")
  Particles#tsparticles(:options="options")
</template>

<script>
import axios from "axios";
import Home from "./components/home.vue";
import Stepper from "./components/stepper.vue";
import options from "./particles.json";
export default {
  name: "App",
  components: { Home, Stepper },
  data() {
    return {
      code: null,
      options: options,
      step: 1,
      display: false,
      delay: true,
      token: null,
      userId: null,
      headers: {
        'Content-type':'application/x-www-form-urlencoded'
      }
    };
  },
  methods: {
    goto(refName) {
      var element = this.$refs[refName];
      var top = element.offsetTop;
      if(this.step == 1){
        window.scroll({ left: 0, top, behavior: "smooth" });
      }
      else{
        window.scroll({ left: 0, top});
      } 
    },
    addStep() {
      this.step += 1;
    },
    gotoStep2() {
      this.step = 2;
    },
    notify(){
      this.display = true;
      setTimeout(() => {
        this.display = false;
      }, 4000)
      setTimeout(() => {
        this.delay = false;
      }, 1000)
    },
    validateCode() {

      var data =  new URLSearchParams ({
            grant_type:"authorization_code",
            code:this.code,
            redirect_uri:"http://localhost:8080/",
            client_id:"SPOTIFY_CLIENT_ID",
            client_secret:"SPOTIFY_CLIENT_SECRET"
      });

      data = data.toString();
      var headers = this.headers;
      var URL= 'https://cors.darpan.online/https://accounts.spotify.com/api/token';
      axios({
        method:'post',
        url:URL,
        data,
        headers:headers
      }).then(res =>{
        this.token = res.data.access_token;
        var header = {
            "Accept": "application/json",
            "Content-Type": "application/json",
            "Authorization": `Bearer ${this.token}`    
        };
        axios({
          method:'get',
          url:'https://api.spotify.com/v1/me',
          headers:header
        }).then(res =>{
            this.userId = res.data.id;
            console.log(res.data);
        }).catch(err =>{
            console.error(err);
        })   
        }).catch(err =>{
            console.error(err);
        })
    },
    updateStep() {
      let urlParams = new URLSearchParams(location.search);
      if (urlParams.get("code")) {
        this.code = urlParams.get("code");
        const url = [
          location.protocol,
          "//",
          location.host,
          location.pathname,
        ].join("");
        window.history.pushState({}, "", url);
      }
      if (this.code !== null) {
        this.step += 1;
        this.validateCode();
      }
    }
  },
  mounted() {
    this.updateStep();
    if(this.step == 2){
      this.goto('stepper');
      this.notify();
    }
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
.bounce-in-top {
	-webkit-animation: bounce-in-top 1s both;
  animation: bounce-in-top 1s both;
}
@-webkit-keyframes bounce-in-top {
  0% {
    -webkit-transform: translateY(-500px);
            transform: translateY(-500px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
    opacity: 0;
  }
  38% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
    opacity: 1;
  }
  55% {
    -webkit-transform: translateY(-65px);
            transform: translateY(-65px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  72% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  81% {
    -webkit-transform: translateY(-28px);
            transform: translateY(-28px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  90% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  95% {
    -webkit-transform: translateY(-8px);
            transform: translateY(-8px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  100% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
}
.bounce-out-top {
	-webkit-animation: bounce-out-top 1s 2s both;
  animation: bounce-out-top 1s 2s both;
}
@-webkit-keyframes bounce-out-top {
  0% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  5% {
    -webkit-transform: translateY(-30px);
            transform: translateY(-30px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  15% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  25% {
    -webkit-transform: translateY(-38px);
            transform: translateY(-38px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  38% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  52% {
    -webkit-transform: translateY(-75px);
            transform: translateY(-75px);
    -webkit-animation-timing-function: ease-in;
            animation-timing-function: ease-in;
  }
  70% {
    -webkit-transform: translateY(0);
            transform: translateY(0);
    -webkit-animation-timing-function: ease-out;
            animation-timing-function: ease-out;
  }
  85% {
    opacity: 1;
  }
  100% {
    -webkit-transform: translateY(-800px);
            transform: translateY(-800px);
    opacity: 0;
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