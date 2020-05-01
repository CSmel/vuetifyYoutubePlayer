<template>
  <div class="black">
    <v-row>
      <video
        id="vid1"
        ref="videoPlayer"
        class="video-js vjs-default-skin vjs-static-controls"
        controls
        height="30px"
        width="600px"
        autoplay
      ></video>
    </v-row>
  </div>
</template>
<script>
import videojs from "video.js";

export default {
  name: "VideoPlayer",
  props: {
    channels_name: String,
    playVideo: Function,
    youtube_det: Function,
    pageTokenUrl: String,
    eventTargetIndex: Number,
    options: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  data() {
    return {
      player: null,
      n: 1,
      videoPlayerIndexProp: this.eventTargetIndex,
      eachVideo: [],
    };
  },
  mounted: function () {
    this.$nextTick(() => {
      this.createVideoArray();
    });

    this.player = videojs(
      this.$refs.videoPlayer,
      this.options,
      function onPlayerReady() {
        console.log("onPlayerReady", this);
      }
    );
  },
  updated() {},
  watch: {
    eventTargetIndex() {
      this.videoPlayerIndexProp = this.eventTargetIndex;
    },
    videoPlayerIndexProp() {
      this.n = this.videoPlayerIndexProp;
    },
  },
  beforeDestroy() {
    if (this.player) {
      this.player.dispose();
    }
  },
  methods: {
    createVideoArray() {
      //this.eachVideo = this.$el.querySelectorAll('.play')
      this.eachVideo = 5;
      console.log(this.eachVideo);
    },
    moveForward(n) {
      this.eachVideo[n - 1].classList.remove("vid-active");
      this.eachVideo[n].classList.add("vid-active");
      this.playMusic(n);
      this.youtube_det();
    },
    moveBackwards(n) {
      this.eachVideo[n - 1].classList.remove("vid-active");
      this.eachVideo[n - 2].classList.add("vid-active");
      this.playMusic(n - 2);
      this.youtube_det();
    },
    playMusic(n) {
      this.videoAttr = this.eachVideo[n].getAttribute("data-vvv");
      this.videoId = this.eachVideo[n].getAttribute("data-vvv");
      //this.pageTokenUrl = "https://www.youtube.com/embed/" + this.videoAttr;
      //document.getElementById("vid1_youtube_api").src = this.pageTokenUrl+"?controls=0&modestbranding=1&rel=0&showinfo=0&loop=0&fs=0&hl=en&iv_load_policy=1&enablejsapi=1&origin=http%3A%2F%2Flocalhost%3A8080&widgetid=1"
    },
  },
};
</script>
<style scoped></style>
