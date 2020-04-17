<template>
  <div>
    <v-card max-width="50%"
            class="mx-auto">
      <v-app-bar
          dark
          color="purple"
      >
        <v-toolbar-title class="my-2">
          <v-btn
              color="primary"
              dark
              @click.stop="dialog = true"
          >
            View {{channelTitle}}'s Information
          </v-btn>
        </v-toolbar-title>
        <v-spacer></v-spacer>
      </v-app-bar>
    </v-card>
    <v-dialog
        v-model="dialog"
        max-width="400"
    >
      <v-card>
        <v-card-title class="headline">{{judul}}</v-card-title>
        <v-card-text>
          <div class="">
            <a
                href="https://www.youtube.com/channel/' + d + '"
                target="_BLANK"
            ><span id="user-img"></span>
              <span class="">{{ channelTitle }}</span></a
            >
            <h4 class="pull-right">{{ viewCount }}</h4>
          </div>
          <div class="">
            <i class="fa fa-clock-o">
              <span>{{ newPublishedAt }}</span></i
            ><i class="fa fa-thumbs-down pull-right"> {{ dislikeCount }}</i
          ><i class="fa fa-thumbs-up pull-right">{{ likeCount }}</i>
            <hr/>
            {{ desc }}<br/>
          </div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
              color="green darken-1"
              text
              @click="dialog = false"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
name: "ContainerPlayer",
comments: {}, data() {
return {
dialog: false,
};
},
props: {
pageTokenUrl: String,
judul: String,
channelTitle: String,
viewCount: String,
newPublishedAt: String,
dislikeCount: String,
likeCount: String,
desc: String
},
computed: {}
}
</script>
<style scoped>
</style>