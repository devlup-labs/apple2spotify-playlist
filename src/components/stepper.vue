<template lang="pug">
.stepper.mx-12.text-left
  div.text-4xl.tracking-wider(v-bind:class = "(step === 1)?'text-white font-bold':'text-green-500 font-semibold'") Step-1
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 1")
    h1.text-white.text-3xl.pb-2 Spotify Login
    p.text-white.tracking-wider.pt-2 Login To Spotify for Converting your Apple Playlist
    button(@click="loggingToSpotify" class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white ")
      span.tracking-widest.px-7.font-bold LOG IN
    button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white" @click="addStep")
      span.tracking-widest.px-7 Next Step
  br
  div.text-4xl.tracking-wider(v-bind:class = "(step === 2)?'text-white font-bold':(step < 2)?'text-gray-600':'text-green-500 font-semibold'") Step-2
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 2")
    div(class="field")
     h1.text-white.text-3xl.mt-10.mb-2.tracking-wide Playlist link
     input(class="text-gray-700 font-bold mb-2 rounded-full w-72 h-7" type="text" v-model="plink" placeholder="Enter the apple playlist link here")
    div(class="mt-5")
     label.text-white.tracking-wider.pt-10 Make your playlist private
     input(class="btn ml-2 h-4 w-4" type="checkbox" v-model="isprivate")
    button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-10 pt-2 pb-3 text-black rounded-full bg-white" @click="addStep")
     span.tracking-widest.pr-7.pl-7.font-bold CONVERT
  br
  div.text-4xl.tracking-wider(v-bind:class = "(step === 3)?'text-white font-bold':(step < 3)?'text-gray-600':'text-green-500 font-semibold'") Step-3
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 3")
    button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white" @click="addStep")
      span.tracking-widest.px-7 Done
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
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
    };
  },
  methods: {
    addStep() {
      this.step += 1;
    },
    loggingToSpotify() {
      var url = `https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.sotifyScopes}&state=34fFs29kd09`;
      window.location.href = url;
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
};
</script>

<style>
.stepper {
  height: 800px;
}
.box{
  margin-left: 20px;
  margin-right: 20px;
  padding: 30px;
}

.field {
  position: relative;
}
.btn {
  position: relative;
}
</style>
