<template>
<v-app id="inspire">
  <v-navigation-drawer app>
    <!-- -->
  </v-navigation-drawer>


  <v-app-bar app>
    <!-- -->
  </v-app-bar>
<v-content>
  <v-container fluid>
  <v-row><ContainerFavorite></ContainerFavorite></v-row>




<v-row>  <container-playlist :channels_title="channels_title" :channels_name="channels_name" :play-video="playVideo" :video-list="videoList"></container-playlist></v-row>




  </v-container>
  <ContainerPlayer :page-token-url="pageTokenUrl"
                   :judul="judul" :channel-title="channelTitle" :view-count="viewCount" :new-published-at="newPublishedAt" :likd="dislikeCount"
                   :likc="likeCount" :desc="desc"></ContainerPlayer>
</v-content>


  <v-footer app       absolute
            class="font-weight-medium">
   <ContainerFooter></ContainerFooter>
  </v-footer>


</v-app>
</template>

<script>
import ContainerPlayer from "./components/ContainerPlayer";
import ContainerPlaylist from "./components/ContainerPlaylist";
import ContainerFavorite from "./components/ContainerFavorite";
import ContainerFooter from "./components/ContainerFooter";
import axios from "axios";
export default {
  name: "App",
  components: {
  ContainerFooter,
  ContainerFavorite,
    ContainerPlayer,
    ContainerPlaylist,
  },
  data() {
    return {
      nekpag: "",
      uploadsId: "",
      channels_title: "",
      channelTitle: "",
      channels_name: "",
      channelsHref: "",
      thumbnailUrl: "",
      newPublishedAt: "",
      viewCount: "",
      likeCount: "",
      dislikeCount: "",
      desc: "",
      judul: "",
      pageTokenUrl: "",
      b: {},
      chid: [],
      videoList: [],
      videoUploads: []
    };
  },
  mounted() {
    this.$nextTick(function() {
      this.connectYoutube();
    });
  },
  methods: {
    connectYoutube() {
      this.channels_name = "808StateOfficial"; //example
      this.channelsHref = "https://www.youtube.com/user/" + this.channels_name;
      this.channels_title = "jQuery plugin by @bachors";
      this.apikey = "AIzaSyChhn0kj1g-rFE69Gb-lRJgbjwtQyKkjp4"; //YOUR GOOGLE API KEY
      this.initUrl =
        "https://www.googleapis.com/youtube/v3/channels?part=contentDetails&forUsername=" +
        this.channels_name +
        "&key=" +
        this.apikey;
      axios
        .get(this.initUrl, {
          crossDomain: true,
          dataType: "json"
        })
        .then(response => {
          this.videoUploads = response.data.items;
          this.uploadsId =
            response.data.items[0].contentDetails.relatedPlaylists.uploads;
          this.chid = response.data.items[0].id;
          this.pageToken = "CAAQAA";
          //console.log(this.uploadsId);
          this.youtube_video_list();

          console.log("video", this.videoUploads);
        });
    },
    youtube_video_list() {
      axios
        .get(
          "https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&playlistId=" +
            this.uploadsId +
            "&key=" +
            this.apikey +
            "&pageToken=" +
            this.pageToken
        )
        .then(response => {
          this.videoList = response.data.items;
          this.videoId = response.data.items[0].snippet.resourceId.videoId;
          this.pageTokenUrl = "https://www.youtube.com/embed/" + this.videoId;
          this.youtube_det();
        });
    },
    youtube_det() {
      axios
        .get(
          "https://www.googleapis.com/youtube/v3/videos?id=" +
            this.videoId +
            "&key=" +
            this.apikey +
            "&part=snippet,statistics"
        )
        .then(response => {
          this.viewCount = response.data.items[0].statistics.viewCount;
          this.likeCount = response.data.items[0].statistics.likeCount;
          this.dislikeCount = response.data.items[0].statistics.dislikeCount;
          this.publishedAt = response.data.items[0].snippet.publishedAt;
          //this.category = ""
          this.judul = response.data.items[0].snippet.localized.title;
          this.desc = response.data.items[0].snippet.localized.description;
          this.channelTitle = response.data.items[0].snippet.channelTitle;
          this.addCommas(this.viewCount);
          this.addCommas(this.likeCount);
          this.addCommas(this.dislikeCount);
          this.urlify(this.desc).replace(/\n/g, "<br />");
          this.newPublishedAt = this.timeSince(
            new Date(this.publishedAt).getTime()
          );
          //this.gplus();
          console.log("video id", this.videoId);
        });
    },
    playVideo(event) {
      if (this.activeLink) {
        this.activeLink.classList.remove("ibacordotcom-vid-active");
      }

      this.activeLink = event.target;
      this.activeLink.classList.add("ibacordotcom-vid-active");
      this.videoAttr = event.target.getAttribute("data-vvv");
      this.videoId = event.target.getAttribute("data-vvv");
      this.videoIndex = event.target.getAttribute("index");
      this.pageTokenUrl = "https://www.youtube.com/embed/" + this.videoAttr;

      console.log(this.videoIndex);
      this.youtube_det();
    },
    addCommas(nStr) {
      nStr += "";
      this.x = nStr.split(".");
      this.x1 = this.x[0];
      this.x2 = this.x.length > 1 ? "." + this.x[1] : "";
      this.rgx = /(\d+)(\d{3})/;
      while (this.rgx.test(this.x1)) {
        this.x1 = this.x1.replace(this.rgx, "$1" + "," + "$2");
        this.viewCount = this.x1 + this.x2;
      }
    },
    timeSince(a) {
      let s = Math.floor((new Date() - a) / 1000),
        i = Math.floor(s / 31536000);
      if (i > 1) {
        return i + " years ago";
      }
      i = Math.floor(s / 2592000);
      if (i > 1) {
        return i + " months ago";
      }
      i = Math.floor(s / 86400);
      if (i > 1) {
        return i + " days ago";
      }
      i = Math.floor(s / 3600);
      if (i > 1) {
        return i + " hours ago";
      }
      i = Math.floor(s / 60);
      if (i > 1) {
        return i + " minutes ago";
      }
      return Math.floor(s) + " seconds ago";
    },
    urlify(b) {
      let c = /(https?:\/\/[^\s]+)/g;
      return b.replace(c, function(a) {
        return (
          '<a href="' + a + '" rel="nofollow" target="_BLANK">' + a + "</a>"
        );
      });
    }
  }
};
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=family=Roboto:100,300,400,500,700,900|Material+Icons');
button {
  height: 200px;
  width: 300px;
  background-color: blue;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
/* Custom your style */

html,
body {

  padding: 0;
  margin: 0;
}

i.fa span {
  font-size: 16px;
}

body * {
  text-shadow: none;
}

#content {
  padding: 0 100px;
}

iframe,
input,
textarea,
button {
  border: 0;
}


</style>
