<template lang="pug">
.stepper.pt-48
  h1.text.text-white.text-3xl.pb-10.mb-10.tracking-wide Spotify Login
  p.text.text-white.tracking-wider.pt-10 Login To Spotify for Converting your Apple this.Playlist
  button(@click="loggingToSpotify" class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-20 pt-2 pb-3 text-black rounded-full bg-white ")
    span.tracking-widest.pr-7.pl-7.font-bold LOG IN
  div(class="field")
   h1.text-white.text-3xl.mt-10.mb-2.tracking-wide this.Playlist link
   input(class="text-gray-700 font-bold mb-2 rounded-full w-72 h-7" type="text" v-model="plink" placeholder="Enter the apple playlist link here")
  div(class="mt-5")
   label.text-white.tracking-wider.pt-10 Make your playlist private
   input(class="btn ml-2 h-4 w-4" type="checkbox" v-model="isprivate")
  button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-10 pt-2 pb-3 text-black rounded-full bg-white" @click="getPlaylistInfoFromApple(plink)")
   span.tracking-widest.pr-7.pl-7.font-bold CONVERT
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      clientId: "SPOTIFY_CLIENT_ID",
      redirectUri: "http://localhost:8080/",
      spotifyScopes:
        "user-read-email playlist-modify-public playlist-modify-private",
      plink: "",
      isprivate: false,
      Playlist: {
        name: "",
        length: 0,
        songs: [],
      },
      playlistLength: 0,
      PlaylistCode: "",
      playlistLenght: null,
      token:
        "Bearer eyJhbGciOiJFUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6IldlYlBsYXlLaWQifQ.eyJpc3MiOiJBTVBXZWJQbGF5IiwiaWF0IjoxNjIyMjUxNTA1LCJleHAiOjE2Mzc4MDM1MDV9.qaZGFoe0pn7-8K0MDbAp2c35nEtrQKz_v4UN2BF4jR7NR2vGKHTzurwsk-rZZUvVjtdqSj5Pli5AKzGn5_OLbQ",
    };
  },
  methods: {
    loggingToSpotify() {
      var url = `https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.sotifyScopes}&state=34fFs29kd09`;
      window.location.href = url;
    },
    getPlaylistInfoFromApple(plink) {
      this.PlaylistCode = plink.split("/");
      this.PlaylistCode = this.PlaylistCode[this.PlaylistCode.length - 1];
      plink =
        "https://cors-anywhere.herokuapp.com/https://amp-api.music.apple.com/v1/catalog/in/playlists/" +
        this.PlaylistCode;

      axios({
        method: "get",
        url: plink,
        headers: { Authorization: this.token },
      }).then(
        (response) => {
          var data = response.data;
          this.Playlist["name"] = data["data"][0]["attributes"]["name"];
          this.playlistLength =
            data["data"][0]["relationships"]["tracks"]["data"].length;
        },
        (error) => {
          console.log(error);
        }
      );
      this.getSongsFromApple();
    },
    async getSongsFromApple(plink) {
      var url = plink;
      while (this.Playlist["length"] % 100 === 0) {
        this.playlistLength = this.Playlist["length"];
        url =
          "https://cors-anywhere.herokuapp.com/https://amp-api.music.apple.com/v1/catalog/in/playlists/" +
          this.PlaylistCode +
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
            this.Playlist["length"] += this.playlistLength;
            for (let i = 0; i < this.playlistLength; i++) {
              let songName = data["data"][i]["attributes"]["name"];
              let artistName = data["data"][i]["attributes"]["artistName"];
              this.Playlist["songs"].push({
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

.field {
  position: relative;
}
.btn {
  position: relative;
}
</style>
