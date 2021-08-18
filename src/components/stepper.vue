<template lang="pug">
.stepper.mx-4.text-left(class="md:mx-12")
  div(class="flex justify-start")
    img.imageHeight(v-if="step === 1" width="40" height="36" alt="Eo circle blue number-1" src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/Eo_circle_blue_number-1.svg/512px-Eo_circle_blue_number-1.svg.png")
    img.imageHeight(v-else src="../../public/icons8-ok.svg", alt="check_mark")
    div.text-4xl.tracking-wider.ml-4(v-bind:class = "(step === 1)?'text-white':'text-gray-500'") Spotify Login
  br
  div.pl-4.border-l-2.ml-4.border-gray-500.py-3
    div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 1")
      p.text-white.tracking-wider.pt-2.text-2xl Login to Spotify for Converting your Apple Playlist
      button(@click="loggingToSpotify" class="button transition duration-100 transform px-2 md:px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white ")
        span.tracking-widest.px-4.font-bold(class="md:px-7") LOG IN
  br
  div(class="flex justify-start")
    img.imageHeight(v-if="step === 1" width="40" height="36" alt="Eo circle grey number-2" src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/32/Eo_circle_grey_number-2.svg/512px-Eo_circle_grey_number-2.svg.png")
    img.imageHeight(v-if="step === 2" width="40" height="36" alt="Eo circle blue number-2" src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/53/Eo_circle_blue_number-2.svg/512px-Eo_circle_blue_number-2.svg.png")
    img.imageHeight(v-if="step === 3" src="../../public/icons8-ok.svg", alt="check_mark")
    div.text-4xl.tracking-wider.ml-4(v-bind:class = "(step === 2)?'text-white':'text-gray-500'") Enter the apple playlist link
  br
  div.pl-4.border-l-2.ml-4.border-gray-500.py-3
    div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 2")
      div(class="btn")
        h1.text-white.text-3xl.font-bold.tracking-wide.mb-4 Playlist Link
        input(class="outline-none border border-transparent focus:outline-none focus:ring-2 focus:ring-green-400 focus:border-transparent text-center text-gray-700 mb-2 text-xs md:text-base rounded-xl w-48 md:px-4 md:w-96 h-10" type="text" v-model="pLink" placeholder="Enter the apple playlist link here")
      div(class="mt-5")
        input(type="checkbox" class="btn form-checkbox mr-2 h-4.5 w-4.5 mb-1 rounded-md text-green-600" checked v-model="isprivate")
        label.text-white.tracking-wider.pt-10.text-xl Make your playlist private
      button(class="transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-10 pt-2 pb-3 text-black rounded-full bg-white" @click="getPlaylistInfoFromApple")
        span.tracking-widest.px-4.font-bold CONVERT
  div(v-if="this.started")
    loader(:render="this.started" :text="this.message")
  br
  div(class="flex justify-start")
    img.imageHeight(v-if="step != 3" width="40" height="36" alt="Eo circle grey number-3" src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/Eo_circle_grey_number-3.svg/512px-Eo_circle_grey_number-3.svg.png")
    img.imageHeight(v-else width="40" height="36" alt="Eo circle blue number-3" src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/Eo_circle_blue_number-3.svg/512px-Eo_circle_blue_number-3.svg.png")
    div.text-4xl.ml-4.tracking-wider(v-bind:class = "(step === 3)?'text-white':'text-gray-500'") Spotify playlist created
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 3")
    div(class="md:flex justify-between")
      button(class="transition duration-100 transform px-6 py-1 mb-4 hover:scale-110 pt-2 pb-3 text-black rounded-full bg-white")
        a.tracking-widest.font-bold(:href="spotifyPlaylistLink") Open in Spotify
      button(class="transition duration-100 transform px-6 py-1 mb-4 hover:scale-110 pt-2 pb-3 text-black rounded-full bg-white " @click="convertAnotherPlaylist")
        span.tracking-widest.font-bold Convert Another Playlist
    iframe.mt-6.rounded-xl(:src="spotifyEmbedPlaylistLink" width="100%" height="380" frameBorder="0" allowtransparency="true" allow="encrypted-media")
    div.mb-6(class = "md:flex justify-between")
      div
        .rounded-full.mt-6.bg-red-400.h-4(class="md:w-72")
          .h-4.bg-green-400.rounded-full(:style = "{ width: percentSongsFound + '%' }")
        p.text-white.text-center.mt-1 {{ percentSongsFound }}% of the songs were converted
      div(class="px-2 mt-6 pt-2 pb-3 text-white rounded-xl hover:underline flex justify-between" style = "cursor:pointer" @click="toggleUnavailableSongs" v-if="percentSongsFound != 100")
        svg(xmlns="http://www.w3.org/2000/svg" v-if="showNotFoundSongs" class="fill-current mt-1 text-white octicon octicon-fold position-relative mr-1" aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true")
          path(d="M10.896 2H8.75V.75a.75.75 0 00-1.5 0V2H5.104a.25.25 0 00-.177.427l2.896 2.896a.25.25 0 00.354 0l2.896-2.896A.25.25 0 0010.896 2zM8.75 15.25a.75.75 0 01-1.5 0V14H5.104a.25.25 0 01-.177-.427l2.896-2.896a.25.25 0 01.354 0l2.896 2.896a.25.25 0 01-.177.427H8.75v1.25zm-6.5-6.5a.75.75 0 000-1.5h-.5a.75.75 0 000 1.5h.5zM6 8a.75.75 0 01-.75.75h-.5a.75.75 0 010-1.5h.5A.75.75 0 016 8zm2.25.75a.75.75 0 000-1.5h-.5a.75.75 0 000 1.5h.5zM12 8a.75.75 0 01-.75.75h-.5a.75.75 0 010-1.5h.5A.75.75 0 0112 8zm2.25.75a.75.75 0 000-1.5h-.5a.75.75 0 000 1.5h.5z")
        svg(xmlns="http://www.w3.org/2000/svg" v-else aria-hidden="true" height="16" viewBox="0 0 16 16" version="1.1" width="16" data-view-component="true" class="fill-current mt-1 text-white octicon octicon-unfold position-relative mr-1")
          path(d="M8.177.677l2.896 2.896a.25.25 0 01-.177.427H8.75v1.25a.75.75 0 01-1.5 0V4H5.104a.25.25 0 01-.177-.427L7.823.677a.25.25 0 01.354 0zM7.25 10.75a.75.75 0 011.5 0V12h2.146a.25.25 0 01.177.427l-2.896 2.896a.25.25 0 01-.354 0l-2.896-2.896A.25.25 0 015.104 12H7.25v-1.25zm-5-2a.75.75 0 000-1.5h-.5a.75.75 0 000 1.5h.5zM6 8a.75.75 0 01-.75.75h-.5a.75.75 0 010-1.5h.5A.75.75 0 016 8zm2.25.75a.75.75 0 000-1.5h-.5a.75.75 0 000 1.5h.5zM12 8a.75.75 0 01-.75.75h-.5a.75.75 0 010-1.5h.5A.75.75 0 0112 8zm2.25.75a.75.75 0 000-1.5h-.5a.75.75 0 000 1.5h.5z")
        span.tracking-widest.px-2(v-if="showNotFoundSongs") Hide Unavailable Songs
        span.tracking-widest.px-2(v-else) Show Unavailable Songs
    div.bg-white.bg-opacity-10.rounded-3xl(v-if="showNotFoundSongs")
      h1.pt-3.ml-4.text-white.text-2xl.font-bold Unavailable Songs:
      li.ml-4.text-white.py-2(v-for="songNotFound in songsNotFound") {{ songNotFound }}
</template>

<script>
import axios from "axios";
import loader from "./loader";

export default {
  props: ["spotifyToken", "step"],
  components: { loader },
  data() {
    return {
      message: "",
      started: false,
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
      songsUri: [],
      songsNotFound: [],
      percentSongsFound: 0,
      playlistLength: 0,
      playlistCode: "",
      playlistId: "",
      token: "apple_token",
      spotifyPlaylistLink: "",
      spotifyEmbedPlaylistLink: "",
      showNotFoundSongs: false,
    };
  },
  methods: {
    convertAnotherPlaylist() {
      this.pLink = "";
      this.isprivate = false;
      this.$emit("gotoStep2", this.step);
      this.percentSongsFound = 0;
      this.songsUri = [];
      this.playlist["length"] = 0;
      this.songsNotFound = [],
      this.playlist["songs"] = [];
    },

    changeMessage() {
      this.started = true;
      this.message = "Extracting songs";
      setTimeout(() => {
        this.message = "Searching songs";
        setTimeout(() => {
          this.message = "Creating playlist";
          setTimeout(() => {
            this.message = "Adding songs";
            setTimeout(() => {
              this.message = "Done";
              setTimeout(() => {
                this.started = false;
                this.$emit("addStep", this.step);
              }, 2000);
            }, 2000);
          }, 2000);
        }, 2000);
      }, 2000);
    },

    toggleUnavailableSongs() {
      this.showNotFoundSongs = !this.showNotFoundSongs;
    },

    loggingToSpotify() {
      var url = `https://accounts.spotify.com/authorize?client_id=${this.clientId}&response_type=code&redirect_uri=${this.redirectUri}&scope=${this.spotifyScopes}&state=34fFs29kd09`;
      window.location.href = url;
    },
    async getPlaylistInfoFromApple() {
      this.playlistCode = this.pLink.split("/");
      this.playlistCode = this.playlistCode[this.playlistCode.length - 1];
      var pLink =
        "https://cors.darpan.online/https://amp-api.music.apple.com/v1/catalog/in/playlists/" +
        this.playlistCode;
      await axios({
        method: "get",
        url: pLink,
        headers: { Authorization: this.token },
      }).then(
        (response) => {
          var data = response.data;
          this.playlist.name = data.data[0]["attributes"]["name"];
          console.log(this.playlist.name);
          this.getSongsFromApple();
        },
        () => {
          console.error();
        }
      );
    },
    async getSongsFromApple() {
      while (this.playlist["length"] % 100 === 0) {
        this.playlistLength = this.playlist["length"];
        var url =
          "https://cors.darpan.online/https://amp-api.music.apple.com/v1/catalog/in/playlists/" +
          this.playlistCode +
          "/tracks/?offset=" +
          this.playlistLength;
        try {
          await axios({
            method: "get",
            url: url,
            headers: { Authorization: this.token },
          }).then((response) => {
            var data = response.data;
            this.playlistLength = data["data"].length;
            this.playlist["length"] += this.playlistLength;
            for (let i = 0; i < this.playlistLength; i++) {
              let songName = data["data"][i]["attributes"]["name"];
              let artistName = data["data"][i]["attributes"]["artistName"];
              let artistArray = artistName.split(", ");
              let last = artistArray[artistArray.length - 1];
              artistArray = artistArray.slice(0, artistArray.length - 1);
              artistArray = artistArray.concat(last.split(" & "));
              this.playlist["songs"].push({
                songname: songName,
                artist: artistArray,
                artistOriginal: artistName,
                songnameTrim: songName.split(" (")[0],
              });
            }
          });
        } catch (error) {
          console.log(error);
          break;
        }
      }
      this.searchingSongsInSpotify();
    },
    async addSongsToPlaylist() {
      let token = this.$parent.token;
      var j = 0;
      var uriArray;
      for (j = 0; j < this.songsUri.length; j + 100) {
        if (this.songsUri.length - j > 100) {
          uriArray = this.songsUri.splice(j, j + 100);
        } else {
          uriArray = this.songsUri.splice(j, this.songsUri.length);
        }
        await axios({
          method: "post",
          url:
            "https://cors.darpan.online/https://api.spotify.com/v1/playlists/" +
            this.playlistId +
            "/tracks/",
          data: JSON.stringify({ uris: uriArray }),
          headers: {
            Authorization: "Bearer " + String(token),
            "Content-type": "application/json",
          },
        })
          .then((res) => {
            console.log(res.data);
          })
          .catch((err) => {
            console.error(err);
          });
      }
    },
    searchingSongsInSpotify() {
      this.songsUri.length = 0;
      this.songsNotFound.length = 0;
      function compare(arr1, arr2) {
        let k = 0;
        let min = Math.min(arr1.length, arr2.length);
        let arr3 = [];
        arr1.forEach((item) => {
          arr3.push(item.name.toUpperCase());
        });
        for (let j = 0; j < arr2.length; j++) {
          arr2[j] = arr2[j].toUpperCase();
        }
        if (arr2.length == min) {
          while (k < min) {
            if (!arr3.includes(arr2[k].toUpperCase())) {
              return false;
            }
            k++;
          }
        } else {
          while (k < min) {
            if (!arr2.includes(arr3[k])) {
              return false;
            }
            k++;
          }
        }
        return true;
      }
      this.playlist["songs"].forEach((song) => {
        let artists = "";
        song.artist.forEach((artist) => {
          artists += artist + " ";
        });
        artists.slice(0, artists.length - 1);
        axios
          .get("https://api.spotify.com/v1/search", {
            headers: {
              Authorization: "Bearer " + this.spotifyToken,
            },
            params: {
              q: "track:" + song.songnameTrim + " artist:" + artists,
              type: "track",
            },
          })
          .then((res) => {
            let items = res.data.tracks.items;
            let i = 0;
            while (items.length > i) {
              if (
                (compare(items[i].artists, song.artist) ||
                  items[i].artists[0].name == song.artistOriginal) &&
                (items[i].name.toUpperCase() ==
                  song.songnameTrim.toUpperCase() ||
                  items[i].name.toUpperCase() == song.songname.toUpperCase())
              ) {
                this.songsUri.push(items[i].uri);
                break;
              }
              i++;
            }
            if (items.length == i) {
              this.songsNotFound.push(song.songname);
            }
          })
          .catch((err) => {
            console.log(err);
          });
      });
      this.createEmptyPlaylist();
    },
    async createEmptyPlaylist() {
      let userId = this.$parent.userId;
      let token = this.$parent.token;
      let playlistId = "";
      let Name = this.playlist.name;
      var data = {
        name: String(Name),
        public: !this.isprivate,
      };
      var headers = {
        "Content-Type": "application/json",
        Authorization: "Bearer  " + String(token),
      };

      await axios
        .post(
          "https://cors.darpan.online/https://api.spotify.com/v1/users/" +
            userId +
            "/playlists",
          JSON.stringify(data),
          { headers }
        )
        .then((res) => {
          playlistId = String(res.data.uri);
          this.spotifyEmbedPlaylistLink =
            "https://open.spotify.com/embed/playlist/" + playlistId.slice(17);
          this.spotifyPlaylistLink =
            "https://open.spotify.com/playlist/" + playlistId.slice(17);
          console.log("playid:", playlistId);
        })
        .catch((err) => err);
      this.$emit("addStep", this.step);
      this.percentSongsFound = Math.floor(
        (this.songsUri.length / this.playlist["length"]) * 100
      );
    },
  },
};
</script>

<style>
.stepper {
  height: 550px;
}
.box {
  margin-left: 20px;
  margin-right: 20px;
  padding: 30px;
}
.btn {
  position: relative;
}
.imageHeight {
  height: 100%;
}
</style>
