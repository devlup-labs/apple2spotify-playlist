<template>
  <div class="container">
    <div class="loading-bar">
      <div class="percentage" v-bind:style="{ width: percentage + '%' }"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "bar",
  data: () => {
    return {
      percentage: 0,
    };
  },

  created() {
    var interval = setInterval(() => {
      if (this.percentage <= 100) this.percentage += 0.1;
      else clearInterval(interval);
    }, 7);
  },
};
</script>

<style>
.loading-bar {
  position: relative;
  width: 400px;
  height: 30px;
  border-radius: 15px;
  overflow: hidden;
  border: 1px solid grey;
}

.percentage {
  position: absolute;
  top: 1px;
  left: 1px;
  right: 1px;
  display: block;
  height: 100%;
  border-radius: 15px;
  background-color: #a5df41;
  background-size: 30px 30px;
}

@keyframes animate-stripes {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: 60px 0;
  }
}
</style>
