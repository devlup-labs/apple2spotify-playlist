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
      span.tracking-widest.px-7.font-bold NEXT
  br
  div.text-4xl.tracking-wider(v-bind:class = "(step === 2)?'text-white font-bold':(step < 2)?'text-gray-600':'text-green-500 font-semibold'") Step-2
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 2")
    div(class="field")
     h1.text-white.text-3xl.mt-10.mb-2.tracking-wide Playlist link
     input(class="text-gray-700 font-bold mb-2 rounded-full w-72 h-7" type="text" v-model="pLink" placeholder="  Enter the apple playlist link here")
    div(class="mt-5")
     label.text-white.tracking-wider.pt-10 Make your playlist private
     input(class="btn ml-2 h-4 w-4" type="checkbox" v-model="isprivate")
    button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-10 pt-2 pb-3 text-black rounded-full bg-white" @click="getPlaylistInfoFromApple")
     span.tracking-widest.pr-7.pl-7.font-bold CONVERT
  div(v-if="this.started")
    loader(:render="this.started" :text="this.message")
  br
  div.text-4xl.tracking-wider(v-bind:class = "(step === 3)?'text-white font-bold':(step < 3)?'text-gray-600':'text-green-500 font-semibold'") Step-3
  br
  div(class="box bg-gray-300 bg-opacity-10 rounded-2xl" v-if="step == 3")
    button(class="button transition duration-100 transform px-6 py-1 m-4 hover:scale-110 mt-8 pt-2 pb-3 text-black rounded-full bg-white" @click="addStep")
      span.tracking-widest.px-7.font-bold DONE
</template>

<script>
import axios from "axios";
import loader from "./loader";

export default {
  props:[
    'spotifyToken','step'
  ],
  components: { loader },
  data() {
    return {
      message: "",
      started: false,
      clientId: "SPOTIFY_CLIENT_ID",
      redirectUri: "http://localhost:8080/",
      spotifyScopes:
        "user-read-email playlist-modify-public playlist-modify-private",
      pLink: '',
      isprivate: false,
      playlist: {
        name: "",
        length: null,
        songs: [],
      },
      songsUri:[],
      songsNotFound:[],
      playlistLength: 0,
      playlistCode: "",
      token: "apple_token",
    };
  },
  methods: {
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
                this.$emit('addStep', this.step);
              }, 2000);
            }, 2000);
          }, 2000);
        }, 2000);
      }, 2000);
    },

    addStep() {
      //this.step += 1;
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
          console.log(this.playlist.name)
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
              let artistArray= artistName.split(', ')
              let last=artistArray[artistArray.length-1]
              artistArray=artistArray.slice(0,artistArray.length-1)
              artistArray=artistArray.concat(last.split(' & '))
              this.playlist["songs"].push({
                songname: songName,
                artist: artistArray,
                artistOriginal:artistName,
                songnameTrim:songName.split(' (')[0]
              });
            }
          },
          (error) => {
            console.log(error);
          }
        );
      }
      this.searchingSongsInSpotify()
    },
    searchingSongsInSpotify() {
      this.songsUri.length=0
      this.songsNotFound.length=0
      function compare(arr1,arr2){
        let k=0
        let min=Math.min(arr1.length,arr2.length)
        let arr3=[]
        arr1.forEach(item=>{
          arr3.push(item.name.toUpperCase())
        })
        for (let j = 0; j < arr2.length; j++) {
          arr2[j] = arr2[j].toUpperCase();
        }    
        if(arr2.length==min){
          while(k<min){
            if(!arr3.includes(arr2[k].toUpperCase())){
              return false
            }
            k++
        }}else{
          while(k<min){
            if(!arr2.includes(arr3[k])){
              return false
            }
            k++
          }                
        }
        return true
      }
      this.playlist["songs"].forEach((song) => {
        let artists=''
        song.artist.forEach((artist)=>{
          artists+=artist+" "
        })
        artists.slice(0,artists.length-1)
        axios
          .get("https://api.spotify.com/v1/search", {
            headers: {
              Authorization: "Bearer " + this.spotifyToken,
            },
            params: {
              q: "track:"+song.songnameTrim+" artist:"+artists,
              type: "track",
            },
          })
          .then((res) => {
            let items = res.data.tracks.items;
            let i = 0
            while( items.length>i) {
              if ((compare(items[i].artists, song.artist) || items[i].artists[0].name== song.artistOriginal ) && (items[i].name.toUpperCase() == song.songnameTrim.toUpperCase() || items[i].name.toUpperCase() == song.songname.toUpperCase() )) {
                this.songsUri.push(items[i].uri);
                break;
              }
              i++
            }
            if(items.length==i ){
              this.songsNotFound.push(song.songname)
            }
          })
          .catch((err)=>{
            console.log(err)
          });
      });
      this.createEmptyPlaylist();
    },
    async createEmptyPlaylist() {
    let userId = this.$parent.userId
    let token = this.$parent.token
    let playlistId = "";
    let Name = this.playlist.name;
    var data = {
    name: String(Name),
    public: !this.isprivate
  }
    var headers = {
      'Content-Type': 'application/json',
      'Authorization': "Bearer  "+ String(token)
    }

    await axios.post(
      "https://cors.darpan.online/https://api.spotify.com/v1/users/"+userId+"/playlists",
      JSON.stringify(data),
      {headers})
      .then(res => {playlistId = String(res.data.uri)
      console.log("playid:", playlistId)})
      .catch(err => err)
  },
  },
};
</script>

<style>
.stepper {
  height: 800px;
}
.box {
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