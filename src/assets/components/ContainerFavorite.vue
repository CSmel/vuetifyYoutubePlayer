<template>
  <v-sheet
      class="mx-auto"
      elevation="8"
      max-width="100%"
  >
    <v-slide-group
        class="pa-4"
        prev-icon="mdi-minus"
        next-icon="mdi-plus"
        show-arrows
    >
      <v-slide-item
          v-for="n in 15"
          :key="n"
          v-slot:default="{ active, toggle }"
      >
        <v-card
            :color="active ? 'primary' : 'grey lighten-1'"
            class="ma-4"
            height="150"
            width="150"
            @click="toggle"
        >
          <v-row
              class="fill-height"
              align="center"
              justify="center"
          >
            <v-scale-transition>
              <v-icon
                  v-if="active"
                  color="white"
                  size="48"
                  v-text="'mdi-close-circle-outline'"
              ></v-icon>
            </v-scale-transition>
          </v-row>
        </v-card>
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
src: 'https://cdn.vuetifyjs.com/images/cards/foster.jpg',
title: 'Supermodel',
artist: 'Foster the People',
},
{
color: '#952175',
src: 'https://cdn.vuetifyjs.com/images/cards/halcyon.png',
title: 'Halcyon Days',
artist: 'Ellie Goulding',
},
],
}),
}
</script>
<style scoped>
</style>