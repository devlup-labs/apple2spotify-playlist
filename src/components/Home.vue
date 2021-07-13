<template lang="pug">
</template>

<script>
export default {
  data() {
    return {
      playlistUri: null,
      step: 1,
      urlParams: null,
    };
  },
  methods: {
    stateCheck() {
      this.urlParams = new URLSearchParams(window.location.search);
      const url = new URL("http://localhost:8080/");
      window.history.pushState({}, "", url);
      if (this.playlistUri !== null) {
        this.step = 3;
      } else if (this.urlParams.get("code")) {
        this.step = 2;
      }
    },
  },
  mounted() {
    this.stateCheck();
  },
};
</script>