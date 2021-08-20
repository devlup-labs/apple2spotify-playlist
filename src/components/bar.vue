<template>
    <div class="loading-bar">
      <div class="percentage" v-bind:style="{ width: String(value) + '%' }"></div>
    </div>
</template>

<script>
export default {
  name: "bar",
  props: ["value"],
  data: () => {
    return {
      percentage: 0,
    };
  },
}
</script>

<style>
.loading-bar {
  position: relative;
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
