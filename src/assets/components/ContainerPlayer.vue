<template>
  <div class="">
    <div class="">
      <iframe
          :src="this.pageTokenUrl"
          allowfullscreen=""
          frameborder="0"
          class="vid-iframe"
      ></iframe>
      <div>
        <div class="">
          <h1>{{ this.judul }}dd</h1>
          <hr />
          <div class="">
            <a
                href="https://www.youtube.com/channel/' + d + '"
                target="_BLANK"
            ><span id="user-img"></span>
              <span class="">{{ this.channelTitle }}</span></a
            >
            <h4 class="pull-right">{{ this.viewCount }}</h4>
          </div>
        </div>
        <div class="">
          <i class="fa fa-clock-o">
            <span>{{ this.newPublishedAt }}</span></i
          ><i class="fa fa-thumbs-down pull-right"> {{ this.dislikeCount }}</i
        ><i class="fa fa-thumbs-up pull-right">{{ this.likeCount }}</i>
          <hr />
          {{ this.desc }}<br />
        </div>
      </div>
    </div>
  </div>
</template>
<script>


export default {
name: "ContainerPlayer",
props:{
pageTokenUrl: String,
judul: String,
channelTitle: String,
viewCount: String,
newPublishedAt: String,
dislikeCount: String,
likeCount: String,
desc: String
}
}
</script>
<style scoped>
</style>