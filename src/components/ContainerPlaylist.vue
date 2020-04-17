<template>
  <div>
    <v-card
        max-width="50%"
        class="mx-auto"
    >
      <v-container>
        <v-row dense>
          <v-col
              v-for="(d, index) in videoList"
              v-bind:key="d.url"
              cols="12"
          >
            <v-hover v-slot:default="{ hover }">
              <v-card
                  height="120"
                  dark
              >
                <div class=" play d-flex flex-no-wrap justify-space-between"
                     :class="{ 'vid-active': index === 0 }"
                     v-bind:index="index"
                     v-bind:data-vvv="d.snippet.resourceId.videoId">
                  <div>
                    <v-card-title
                        class="headline"
                    >
                      <div
                          :min="d.minute"
                      >
                        <div>
                          <span :class="[`vid-tm${index}`] "></span>
                        </div>
                        {{ d.snippet.title }}
                      </div>
                    </v-card-title>
                    <v-card-subtitle><span class="displayDuration">
  <span class="min"></span>:<span class="sec"></span>
</span></v-card-subtitle>
                  </div>
                  <v-avatar
                      class="ma-3"
                      size="100"
                      tile
                  >
                    <v-img :src="d.snippet.thumbnails.default.url"></v-img>
                  </v-avatar>
                  <div class="durationId" :class="[`duration${index}`] ">
                    {{d.snippet.resourceId.videoId}}
                  </div>
                </div>
                <v-expand-transition>
                  <div
                      @click.prevent="playVideo"
                      v-if="hover"
                      class="d-flex pointer transition-fast-in-fast-out orange darken-2 v-card--reveal display-3 white--text"
                      style="height: 100%;"
                  >
                    Play Me
                  </div>
                </v-expand-transition>
              </v-card>
            </v-hover>
          </v-col>
        </v-row>
      </v-container>
    </v-card>
  </div>
</template>
<script>
import axios from "axios";

export default {
name: "ContainerPlaylist",
data() {
return {
newId: '',
videoDuration: '',
seconds: '',
minute: '',
videoData: '',
contentDetailsArray: [],
}
},
props: {
videoList: Array,
playVideo: Function,
channels_title: String,
channels_name: String,
channelsHref: String,
},
updated() {

this.formatDurationTime()

},
methods: {
formatDurationTime() {
this.vidTotal = this.$el.querySelectorAll('.play');
for (let i = 0; i < this.vidTotal.length; i++) {
this.newId = this.$el.getElementsByClassName('durationId')[i].innerHTML;
axios
  .get(
    "https://www.googleapis.com/youtube/v3/videos?id=" + this.newId + "&key=AIzaSyChhn0kj1g-rFE69Gb-lRJgbjwtQyKkjp4&part=contentDetails"
  )
  .then(response => {
  this.contentDetailsArray = response.data.items;
  this.videoDuration = response.data.items[0].contentDetails.duration;
  if (this.videoDuration.match(/M/g)) {
  this.videoData = this.videoDuration.split('M');
  this.minute = this.videoData[0].replace('PT', '');
  if (this.videoData[1] != '') {
  this.seconds = this.videoData[1].replace('S', '');
  } else {
  this.seconds = '00';
  }
  } else {
  this.videoData = this.videoDuration.split('PT');
  this.minute = '00';
  this.seconds = this.videoData[1].replace('S', '');
  }

  this.$el.getElementsByClassName('min')[i].innerHTML = this.minute;
  this.$el.getElementsByClassName('sec')[i].innerHTML = this.seconds
  })
}

},
}
}
</script>
<style scoped>
  .vid-active {
    background-color: red !important;
  }

  .durationId {
    border: 1px solid red;
    display: none;
  }

  .displayDuration {
    color: white;
  }

  .v-card--reveal {
    align-items: center;
    bottom: 0;
    justify-content: center;
    opacity: .5;
    position: absolute;
    width: 100%;
  }

  .pointer {
    cursor: pointer;
  }
</style>