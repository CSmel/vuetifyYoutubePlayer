<template>
  <div>
    {{ this.channels_name }}
    {{ this.playlistIndex }}
    <v-card max-width="50%" class="mx-auto">
      <v-app-bar dark color="purple"
        ><br />
        <v-chip color="purple"> Playlist: {{ playlistTitle }} </v-chip>
        <v-spacer></v-spacer>
        <v-toolbar-title class="my-2">
          <div class="black">
            <v-chip
              color="black"
              @click="playlistIndex--"
              :disabled="playlistIndex <= 0"
              class="ma-2"
            >
              <v-icon small color="grey darken-1">$backward</v-icon>
            </v-chip>
            <v-chip
              color="black"
              @click="playlistIndex++"
              :disabled="playlistIndex >= this.playlistIdArray.length"
              class="ma-2"
            >
              <v-icon small color="grey darken-1">$next</v-icon>
            </v-chip>
          </div>
        </v-toolbar-title>
      </v-app-bar>
    </v-card>
  </div>
</template>
<script>
export default {
  name: "ContainerPlayer",
  comments: {},
  data() {
    return {
      playlistIndex: 0,
      dialog: false,
    };
  },
  props: {
    channels_name: String,
    pageTokenUrl: String,
    title: String,
    playlistTitle: String,
    channelTitle: String,
    viewCount: String,
    newPublishedAt: String,
    dislikeCount: String,
    likeCount: String,
    desc: String,
    playlistIdArray: Array,
  },
  updated() {
    this.$emit("update:playlistIndex", this.playlistIndex);
  },
  watch: {
    channels_name() {
      this.playlistIndex = 0;
    },
  },
  computed: {},
  methods: {},
};
</script>
<style scoped></style>
