<template lang="pug">
#app
  .text.mx-auto
    nav.p-12.text-center.font-semibold.text-5xl.text-white(class="md:text-7xl")
      h1.tracking-in-contract-bck Playlistify
    Home(@clicked="goto('stepper')")
    .my-80(ref="stepper")
      Stepper(:step = "step")
  Particles#tsparticles(:options="options")
</template>

<script>
//- import axios from "axios";
import Home from "./components/home.vue";
import Stepper from "./components/stepper.vue";
import options from "./particles.json";
export default {
  name: 'App',
  components: { Home, Stepper },
  data() {
    return {
      code: null,
      options: options,
      step: 1,
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
    updateStep() {
      let urlParams = new URLSearchParams(location.search);
      if (urlParams.get("code")) {
        this.code = urlParams.get("code");
        const url = [location.protocol, '//', location.host, location.pathname].join('');
        window.history.pushState({}, "", url);
      }
      if (this.code !== null) {
        this.step += 1;
      }
    }
  },
  mounted() {
    this.updateStep();
    if(this.step == 2){
      this.goto('stepper');
    }
  },
};
</script>

<style>
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