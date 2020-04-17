<template>
  <v-sheet
      class="mx-auto"
      elevation="8"
      max-width="100%"
  >
    <v-slide-group
        class="pa-4"
        next-icon="mdi-plus"
        prev-icon="mdi-minus"
        show-arrows
    >
      <v-slide-item
          :key="n.src"
          v-for="n in items"
          v-slot:default="{ active, toggle }"
      >
        <v-tooltip bottom>
          <template v-slot:activator="{ on }">
            <v-card
                :artistChannelName="n.artistChannelName"
                :color="n.color"
                @click="toggle"
                @click.prevent="updateArtist"
                class="ma-4"
                height="150"
                v-on="on"
                width="150"
            >
              <v-img :src="n.src"></v-img>
              <v-row
                  align="center"
                  class="fill-height"
                  justify="center"
              >
                <v-scale-transition>
                  <v-icon
                      size="48"
                      v-if="active"
                      v-text="'mdi-close-circle-outline'"
                  ></v-icon>
                </v-scale-transition>
              </v-row>
            </v-card>
          </template>
          <span>{{n.artist}}</span>
        </v-tooltip>
      </v-slide-item>
    </v-slide-group>
  </v-sheet>
</template>
<script>
export default {
name: "ContainerFavorite",
data: () => ({
items: [
{
color: '#1F7087',
src: 'https://yt3.ggpht.com/a/AATXAJxC_8OjSN8NLtxFctXu2X49PSZSqQ_hnCdwmw=s288-c-k-c0xffffffff-no-rj-mo',

artist: 'EricPrydzTV',
artistChannelName: 'EricPrydzTV'
},
{
color: '#952175',
src: 'https://yt3.ggpht.com/a/AATXAJz8OfzGwM0UBNE-5XEhPWiqYkWxpMZEESUMYA=s288-c-k-c0xffffffff-no-rj-mo',
artist: 'Above & Beyond',
artistChannelName: 'aboveandbeyondtv',
},
{
color: '#952175',
src: 'https://yt3.ggpht.com/a/AATXAJxtFIhLe_LJoEWGhDBqFsYlnzVzGtHxbSMpDg=s288-c-k-c0xffffffff-no-rj-mo',
artist: 'Halcyon Days',
artistChannelName: 'officialkmfdm',
}, {
color: '#952175',
src: 'https://yt3.ggpht.com/a/AATXAJz_Bi93s6_FkisMbGX8lSa5t2RIlExAjE5QPQ=s288-c-k-c0xffffffff-no-rj-mo',
artist: 'Orbital',
artistChannelName: 'oorbitalofficial',
}, {
color: '#952175',
src: 'https://yt3.ggpht.com/a/AATXAJwWNQ0z_qIgPQXjQ8aXh1MpqGOxDSNvJ-jMiQ=s288-c-k-c0xffffffff-no-rj-mo',
artist: 'Deftones',
artistChannelName: 'deftones',
},
{
color: '#952175',
src: 'https://yt3.ggpht.com/a/AATXAJxLaykUXJSIpgPIpR_hK7T-GpCzkMr5ZXPLkQ=s288-c-k-c0xffffffff-no-rj-mo',
artist: 'Massive Attack',
artistChannelName: 'massiveattack',
},
],
}),
props: {
updateArtist: Function
}
,
methods: {}
}
</script>
<style scoped>
</style>