<template>
  <div class="container">
    <div class="loading-bar">
      <div class="percentage" v-bind:style="{ width: percentage + '%' }"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["value"],
  name: "bar",
  data: () => {
    return {
      percentage: 0,
    }
  },
  computed: {
   created() {
      return this.percentage.toFixed();
    }
   },
   created() {

    var intval = setInterval(() => {
      if(this.percentage < 100)
        this.percentage += this.value;
      else
        clearInterval(intval);
    }, 2000);
   }
}
</script>

<style>
.loading-bar {
  position: relative;
  width: 400px;
  height: 30px;
  border-radius: 15px;
  overflow: hidden;
  border: 1px solid grey;
  box-shadow: inset 0 1px 2px rgba($color: #000, $alpha: .4), 
    0 -1px 1px #fff, 0 1px 0 #fff;
}

.percentage {
  position: absolute;
  top: 1px; left: 1px; right: 1px;
  display: block;
  height: 100%;
  border-radius: 15px;
  background-color: #a5df41;
  background-size: 30px 30px;
  background-image: linear-gradient(135deg, rgba($color: #fff, $alpha: .15) 25%, transparent 25%,
    transparent 50%, rgba($color: #fff, $alpha: .15) 50%,
    rgba($color: #fff, $alpha: .15) 75%, transparent 75%,
    transparent); 
  animation: animate-stripes 3s linear infinite;
}

@keyframes animate-stripes {
  0% { background-position: 0 0; }
  100% { background-position: 60px 0; }
}
</style>
