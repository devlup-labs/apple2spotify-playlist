<template lang="pug">
div(class=" md:flex justify-between gap-24")
  div(class="flex justify-center md:justify-start mx-auto")
    div(class=" mr-0 text-gray-300 text-3xl text-center md:text-left md:text-5xl")
      h2.mt-8 Convert an
      h2.mt-8 Apple playlist
      h2.mt-8 into a Spotify playlist.
      button(@click="goto('stepper')" class=" mt-16 pt-2 pb-3 px-4 text-black hover:shadow-inner hover:text-black hover-effect text-center text-lg font-semibold rounded-full")
        span.mx-4 Convert Playlist
  div
    img(class="mt-6 mx-auto md:mt-0 md:h-96 md:mr-40"
      src="../../public/logo-trans1.gif"
      alt="logo-transformation")
div.mt-80(ref="stepper")
  Stepper
</template>

<script>
import Stepper from './stepper.vue'
export default {
    components: { Stepper },
    methods: {
        goto(refName) {
        var element = this.$refs[refName];
        var top = element.offsetTop;

        window.scroll({left:0, top, behavior: 'smooth'});
        }
    }
}
</script>

<style>
.hover-effect {
  background-color: #1ed760;
}
.hover-effect:hover {
  background-color: #2de26d;
  transform: scale(1.04, 1.04);
}
</style>