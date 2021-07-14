<template>
  <div>
    <center>
      <h1>Playlistify</h1>
      <div id="button">
      <button v-on:click="change">Convert</button>
      </div>
      <br>
      <loader :clicked="this.started" />
      <statusChanger :message="this.message" />
    </center>  
  </div>
</template>

<script>
  import loader from "./components/loader"
  import statusChanger from "./components/statusChanger"

  export default {
    name: 'App',
    components: {
      loader,
      statusChanger,
    },

    data(){
      return {message: "", started: false}
    },


    methods: {
      change: function () {
        this.started = true;
        this.message = "Extracting from Apple Playlist"
        setTimeout(() => {
          this.message = "Creating Playlist",
          setTimeout(() => {
            this.message = "Adding Songs",
            setTimeout(() => {
              this.message = "Done",
              this.started = false
            }, 2000)
          }, 2000)
        }, 2000)
      },
    }
  }
</script>


<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    background-color: white;
    color: black;
    margin-top: 60px;
  } 

  #button {
    padding-top: 20px;
  }
</style>
