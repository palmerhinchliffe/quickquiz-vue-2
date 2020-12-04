<template>
  <v-card
    class="mx-auto"
  >
    <v-toolbar color="dark accent-4" dark>
      <v-toolbar-title>Settings</v-toolbar-title>
    </v-toolbar>
    
    <v-card-text>
      <v-slider
        class="my-10"
        label="Difficulty:"
        :max="3"
        step="1"
        ticks="always"
        tick-size="2"
      ></v-slider>
      <v-slider
        class="my-8"
        label="Questions:"
        :min="5"
        step="5"
        :max="20"
        thumb-label="always"
        :thumb-size="20"
      ></v-slider>
      <v-switch
        label="Timer off"
      ></v-switch>
      <v-slider
        class="my-8"
        label="Timer (seconds):"
        step="5"
        min="5"
        max="30"
        thumb-label="always"
        :thumb-size="20"
      ></v-slider>
    </v-card-text>
  </v-card>
</template>
