<template lang='pug'>
.stepper.pt-48
  h1.text.text-white.text-3xl.pb-10.mb-10.tracking-wide Spotify Login
  p.text.text-white.tracking-wider.pt-10 Login To Spotify for Converting your Apple Playlist
  button(@click="loggingToSpotify" class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-20 pt-2 pb-3 text-black rounded-full bg-white ")
    span.tracking-widest.pr-7.pl-7.font-bold LOG IN
  div(class="field")
   h1.text-white.text-3xl.mt-10.mb-2.tracking-wide Playlist link
   input(class="text-gray-700 font-bold mb-2 rounded-full w-72 h-7" type="text" v-model="plink" placeholder="Enter the apple playlist link here")
  div(class="mt-5")
   label.text-white.tracking-wider.pt-10 Make your playlist private
   input(class="btn ml-2 h-4 w-4" type="checkbox" v-model="isprivate")
  button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-10 pt-2 pb-3 text-black rounded-full bg-white" v-on:click="change")
   span.tracking-widest.pr-7.pl-7.font-bold CONVERT
  div(v-if="this.started")
   loader(:render="this.started" :text="this.message" :value="this.value")
</template>

<script>
import loader from "./loader"

export default {
  components: {loader},
  data(){
    return {
      step: 1,
      message: "",
      value: 0,
      started: false,
      step: 1, 
      clientId: "SPOTIFY_CLIENT_ID",
      redirectUri: "http://localhost:8080/",
      spotifyScopes:
        "user-read-email playlist-modify-public playlist-modify-private",
      pLink: "",
      isprivate: false,
      playlist: {
        name: "",
        length: null,
        songs: [],
      },
      playlistLength: 0,
      playlistCode: "",
      token: "appleToken",
    }
  },
  methods:{
    addStep() {
      this.step += 1;
    },
    
    loggingToSpotify() { 
      var url =`https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.sotifyScopes}&state=34fFs29kd09`;
      window.location.href = url; 
    },

    change() {
      this.started = true;
      this.message = "Extracting"
      setTimeout(() => {
        this.value+=30
        this.message = "Creating Playlist",
        setTimeout(() => {
          this.value+=30
          this.message = "Adding Songs",
          setTimeout(() => {
            this.value+=40
            this.message = "Done",
            setTimeout(() => {
              this.started = false;
              this.value=0;
            }, 2000)
          }, 2000)
        }, 2000)
      }, 2000)
    },
    
    getPlaylistInfoFromApple(pLink) {
      this.playlistCode = pLink.split("/");
      this.playlistCode = this.playlistCode[this.playlistCode.length - 1];
      pLink =
        "https://cors.darpan.online/https://amp-api.music.apple.com/v1/catalog/in/playlists/" +
        this.playlistCode;
      axios({
        method: "get",
        url: pLink,
        headers: { Authorization: this.token },
      }).then(
        (response) => {
          var data = response.data;
          this.playlist["name"] = data["data"][0]["attributes"]["name"];
          this.playlist["length"] =
            data["data"][0]["relationships"]["tracks"]["data"].length;
        },
        () => {
          console.error();
        }
      );
      this.getSongsFromApple();
    },
    async getSongsFromApple() {
      while (this.playlist["length"] % 100 === 0) {
        this.playlistLength = this.playlist["length"];
        var url =
          "https://cors.darpan.online/https://amp-api.music.apple.com/v1/catalog/in/playlists/" +
          this.playlistCode +
          "/tracks/?offset=" +
          this.playlistLength;
        await axios({
          method: "get",
          url: url,
          headers: { Authorization: this.token },
        }).then(
          (response) => {
            var data = response.data;
            this.playlistLength = data["data"].length;
            this.playlist["length"] += this.playlistLength;
            for (let i = 0; i < this.playlistLength; i++) {
              let songName = data["data"][i]["attributes"]["name"];
              let artistName = data["data"][i]["attributes"]["artistName"];
              this.playlist["songs"].push({
                songname: songName,
                artist: artistName,
              });
            }
          },
          (error) => {
            console.log(error);
          }
        );
      }
    }, 
  },
}
</script>

<style>
.stepper{
    height: 800px;
}

.field{
  position:relative;
}
.btn{
  position:relative;
}

</style>
