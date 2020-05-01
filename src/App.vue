<template>
  <v-app id="inspire">
    <v-content class="grey darken-4">
      <v-container fluid>
        <v-row>
          <ContainerFavorite
            class="grey darken-4"
            :update-artist="updateArtist"
          ></ContainerFavorite>
        </v-row>
        <v-col class="grey darken-4 text-center">
          <ContainerPlayer
            :playlistIndex.sync="appPlaylistIndex"
            :playlistIdArray="playlistIdArray"
            :page-token-url="pageTokenUrl"
            :playlistTitle="playlistTitle"
            :title="title"
            :channel-title="channelTitle"
            :view-count="viewCount"
            :new-published-at="newPublishedAt"
            :likd="dislikeCount"
            :channels_name="channels_name"
            :likc="likeCount"
            :desc="desc"
          ></ContainerPlayer>
          <container-playlist
            :visible="visible"
            :playlistIdArray="playlistIdArray"
            :page-token-url="pageTokenUrl"
            :playlistTitle="playlistTitle"
            :title="title"
            :channel-title="channelTitle"
            :view-count="viewCount"
            :new-published-at="newPublishedAt"
            :likd="dislikeCount"
            :likc="likeCount"
            :desc="desc"
            :play-video="playVideo"
            :channels_title="channels_title"
            :channels_name="channels_name"
            :video-list="videoList"
          ></container-playlist>
        </v-col>
      </v-container>
    </v-content>
    <v-footer app fixed class="font-weight-medium black">
      <video-player
        :event-target-index="eventTargetIndex"
        :youtube_det="youtube_det"
        :page-token-url="pageTokenUrl"
        :options="videoOptions"
      />
      <ContainerFooter></ContainerFooter>
    </v-footer>
  </v-app>
</template>
<script>
import ContainerPlayer from "./components/ContainerPlayer";
import ContainerPlaylist from "./components/ContainerPlaylist";
import ContainerFavorite from "./components/ContainerFavorite";
import ContainerFooter from "./components/ContainerFooter";
import VideoPlayer from "./components/VideoPlayer.vue";

import axios from "axios";
import videojs from "video.js";
//import videojs from 'video.js/dist/video.js'

export default {
  name: "App",
  components: {
    ContainerFooter,
    ContainerFavorite,
    ContainerPlayer,
    ContainerPlaylist,
    VideoPlayer,
  },
  data() {
    return {
      videoId: "ehNXOIpRr6c",
      appPlaylistIndex: "",
      eventTargetIndex: 1,
      pageToken: "",
      uploadsId: "",
      channels_title: "",
      channelTitle: "",
      channels_name: "ninofficial",
      channelsHref: "",
      thumbnailUrl: "",
      newPublishedAt: "",
      viewCount: "",
      likeCount: "",
      dislikeCount: "",
      desc: "",

      title: "",
      playlistTitle: "",
      pageTokenUrl: "",
      nextpageToken: "",
      channelId: [],
      videoList: [],
      videoUploads: [],
      visible: false,
      playlistIdArray: [],
      videoOptions: {
        loadingSpinner: false,
        autoplay: false,
        controlBar: {
          fullscreenToggle: false,
          disablePictureInPicture: true,
        },
        sources: [
          {
            src: "https://www.youtube.com/embed/ehNXOIpRr6c",
            type: "video/youtube",
          },
        ],
      },
    };
  },
  props: {
    playlistIndex: {
      type: Number,
      default: 0,
    },
  },
  watch: {
    appPlaylistIndex() {
      this.connectYoutube();
    },
  },
  updated() {},
  mounted() {
    this.$nextTick(function () {
      this.connectYoutube();
    });
  },
  methods: {
    // 1 connect
    connectYoutube() {
      //this.channels_name = "massiveattack"; //example
      this.channelsHref = "https://www.youtube.com/user/" + this.channels_name;
      this.channels_title = "jQuery plugin by @bachors";
      this.apikey = "AIzaSyChhn0kj1g-rFE69Gb-lRJgbjwtQyKkjp4"; //YOUR GOOGLE API KEY
      this.initUrl =
        "https://www.googleapis.com/youtube/v3/channels?part=contentDetails&snippet&contentOwnerDetails&forUsername=" +
        this.channels_name +
        "&key=" +
        this.apikey;
      axios
        .get(this.initUrl, {
          crossDomain: true,
          dataType: "json",
        })
        .then((response) => {
          this.videoUploads = response.data;
          this.uploadsId =
            response.data.items[0].contentDetails.relatedPlaylists.uploads;
          this.channelId = response.data.items[0].id;
          this.pageToken = "";
          this.getPlaylistId();
        });
    },

    // ORIGINAL VIDEO LIST BY UPLOADS ID
    // youtube_video_list() {
    // axios
    //  .get(
    //     "https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&playlistId=" +
    //    this.uploadsId +
    //     "&key=" +
    //    this.apikey + "&pageToken="
    //    + this.pageToken
    //   )
    //   .then(response => {
    //   this.nextpageToken = response.data.nextPageToken;
    //
    //   this.videoList = response.data.items;
    //   this.videoId = response.data.items[0].snippet.resourceId.videoId;
    //   this.pageTokenUrl = "https://www.youtube.com/embed/g1rz_QXQt34" + this.videoId;
    //   this.youtube_det(response.data);
    //   });
    // },

    getPlaylistId() {
      axios
        .get(
          "https://www.googleapis.com/youtube/v3/playlists?part=snippet&maxResults=50&channelId=" +
            this.channelId +
            "&key=" +
            this.apikey +
            "&pageToken="
          // this.pageToken
        )
        .then((response) => {
          this.playlistIdArray = response.data.items;
          this.playlistTitle =
            response.data.items[this.appPlaylistIndex].snippet.title;
          this.playListId = response.data.items[this.appPlaylistIndex].id;
          console.log("Get Playlist Id", this.appPlaylistIndex);

          this.displayPlayListIdList();
        });
    },

    displayPlayListIdList() {
      axios
        .get(
          "https://www.googleapis.com/youtube/v3/playlistItems?part=snippet&maxResults=50&key=" +
            this.apikey +
            "&playlistId=" +
            this.playListId
          // this.pageToken
        )
        .then((response) => {
          this.videoList = response.data.items;
          this.videoId = response.data.items[0].snippet.resourceId.videoId;
          this.pageTokenUrl =
            "https://www.youtube.com/embed/g1rz_QXQt34" + this.videoId;
          this.displayPlayListIdListArray = response.data;
          console.log("Video List", this.videoList);
          this.youtube_det();
        });
    },

    // detect which video is active abd play it.
    youtube_det() {
      axios
        .get(
          "https://www.googleapis.com/youtube/v3/videos?id=" +
            this.videoId +
            "&key=" +
            this.apikey +
            "&part=snippet,statistics"
        )
        .then((response) => {
          this.viewCount = response.data.items[0].statistics.viewCount;
          this.likeCount = response.data.items[0].statistics.likeCount;
          this.dislikeCount = response.data.items[0].statistics.dislikeCount;
          this.publishedAt = response.data.items[0].snippet.publishedAt;
          //this.category = ""
          this.title = response.data.items[0].snippet.localized.title;
          this.desc = response.data.items[0].snippet.localized.description;
          this.channelTitle = response.data.items[0].snippet.channelTitle;
          this.addCommas(this.viewCount);
          this.addCommas(this.likeCount);
          this.addCommas(this.dislikeCount);
          this.urlify(this.desc).replace(/\n/g, "<br />");
          this.newPublishedAt = this.timeSince(
            new Date(this.publishedAt).getTime()
          );
        });
    },

    playVideo(event) {
      if (this.activeLink) {
        this.activeLink.classList.remove("vid-active");
      }
      let els = document.querySelectorAll(".play.vid-active");
      this.playClassTotal = document.querySelectorAll(".play");
      this.eventTargetIndex =
        Array.from(this.playClassTotal).indexOf(event.target) + 1;
      for (let i = 0; i < els.length; i++) {
        els[i].classList.remove("vid-active");
      }
      this.activeLink = event.target;
      this.activeLink.previousElementSibling.classList.add("vid-active");
      this.videoAttr = event.target.previousElementSibling.getAttribute(
        "data-vvv"
      );
      this.videoId = event.target.previousElementSibling.getAttribute(
        "data-vvv"
      );
      this.pageTokenUrl = "https://www.youtube.com/embed/" + this.videoAttr;
      //this.youtubeSrc = this.pageTokenUrl + "?controls=0&modestbranding=1&rel=0&showinfo=0&loop=0&fs=0&hl=en&iv_load_policy=1&enablejsapi=1&origin=http%3A%2F%2Flocalhost%3A8080&widgetid=1";
      //document.getElementById("vid1_youtube_api").src = this.pageTokenUrl + "?controls=0&modestbranding=1&rel=0&showinfo=0&loop=0&fs=0&hl=en&iv_load_policy=1&enablejsapi=1&origin=http%3A%2F%2Flocalhost%3A8080&widgetid=1";
      let ytplayer = videojs("vid1");
      ytplayer.src({ type: "video/youtube", src: this.pageTokenUrl });
      console.log(this.videoId);
      this.youtube_det();
    },
    updateArtist(event) {
      this.channels_name = event.currentTarget.getAttribute(
        "artistChannelName"
      );
      this.connectYoutube();
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
      return b.replace(c, function (a) {
        return (
          '<a href="' + a + '" rel="nofollow" target="_BLANK">' + a + "</a>"
        );
      });
    },
  },
};
</script>
<style src="video.js/dist/video-js.css">
/* global styles */
</style>
<style>
@import url("https://fonts.googleapis.com/css?family=family=Roboto:100,300,400,500,700,900|Material+Icons");
button {
  height: 200px;
  width: 300px;
  background-color: blue;
}

#vid1 {
  margin-top: 10px;
}
.vjs-default-skin.vjs-paused .vjs-big-play-button {
  display: none;
}

.vjs-default-skin.vjs-paused .vjs-control-bar {
  display: flex;
  width: 600px;
}
.vjs-default-skin.vjs-has-started .vjs-control-bar {
  visibility: visible !important;
  opacity: 1 !important;

  background-color: rgba(7, 20, 30, 1) !important;
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

iframe,
input,
textarea,
button {
  border: 0;
}

.vid-active {
  background-color: #78909c;
}
</style>