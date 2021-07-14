<template lang="pug">
.text-focus-in.mb-60(class="md:flex justify-between gap-24")
  .flex.mx-auto(class="justify-center md:justify-start")
    .mr-0.text-gray-300.text-3xl.text-center(class="md:text-left md:text-5xl")
      h2.mt-8 Convert an
      h2.mt-8 Apple playlist
      h2.mt-8 into a Spotify playlist.
      button(@click = "emitX" class=" mt-16 pt-2 pb-3 px-4 text-black hover:shadow-inner button hover:text-black text-center text-lg font-semibold rounded-full")
        span.mx-4 Convert Playlist
  div
    img(class="mt-6 mx-auto md:mt-0 md:h-96 md:mr-40"
      src="../../public/logo-trans1.gif"
      alt="logo-transformation")
</template>

<script>
export default {
  methods: {
    emitX() {
      this.$emit("clicked");
    },
  },
};
</script>

<style>
h2 {
  font-family: "Varela Round", sans-serif;
}

.button {
  z-index: 1;
  background-color: #1ed760;
  -webkit-animation: shake-top 1.5s cubic-bezier(0.455, 0.03, 0.515, 0.955) both;
  animation: shake-top 1.5s cubic-bezier(0.455, 0.03, 0.515, 0.955) both;
  animation-delay: 2s;
}

@-webkit-keyframes shake-top {
  0%,
  100% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
    -webkit-transform-origin: 50% 0;
    transform-origin: 50% 0;
  }
  10% {
    -webkit-transform: rotate(2deg);
    transform: rotate(2deg);
  }
  20%,
  40%,
  60% {
    -webkit-transform: rotate(-4deg);
    transform: rotate(-4deg);
  }
  30%,
  50%,
  70% {
    -webkit-transform: rotate(4deg);
    transform: rotate(4deg);
  }
  80% {
    -webkit-transform: rotate(-2deg);
    transform: rotate(-2deg);
  }
  90% {
    -webkit-transform: rotate(2deg);
    transform: rotate(2deg);
  }
}

button:hover {
  background-color: #2de26d;
  cursor: pointer;
  animation: jelly 0.5s;
}

@keyframes jelly {
  0%,
  100% {
    transform: scale(1, 1);
  }
  25% {
    transform: scale(0.9, 1.1);
  }
  50% {
    transform: scale(1.1, 0.9);
  }
  75% {
    transform: scale(0.95, 1.05);
  }
}

.text-focus-in {
  -webkit-animation: text-focus-in 2s cubic-bezier(0.55, 0.085, 0.68, 0.53) both;
  animation: text-focus-in 2s cubic-bezier(0.55, 0.085, 0.68, 0.53) both;
}

@-webkit-keyframes text-focus-in {
  0% {
    -webkit-filter: blur(12px);
    filter: blur(12px);
    opacity: 0;
  }
  100% {
    -webkit-filter: blur(0px);
    filter: blur(0px);
    opacity: 1;
  }
}
</style>
