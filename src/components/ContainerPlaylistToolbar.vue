<template>
  <div>
    <v-card
      :fullscreen="$vuetify.breakpoint.xsOnly"
      max-width="700px"
      class="mx-auto"
    >
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
  name: "ContainerPlaylistToolbar",
  comments: {},
  data() {
    return {
      playlistIndex: 0,
      dialog: false,
    };
  },
  props: {
    playlistTitle: String,
    playlistIdArray: Array,
  },
  updated() {
    this.$emit("update:playlistIndex", this.playlistIndex);
  },
  watch: {
    playlistTitle() {
      this.playClassTotal = document.querySelectorAll(".play");
      this.eventTargetIndex = Array.from(this.playClassTotal);
      for (let i = 0; i < this.playClassTotal.length; i++) {
        this.playClassTotal[i].classList.remove("vid-active");
        this.playClassTotal[0].classList.add("vid-active");
      }
    },
  },
  computed: {},
  methods: {},
};
</script>
<style scoped></style>
