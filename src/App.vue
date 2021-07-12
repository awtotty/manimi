<template>
  <v-app id="inspire">
    <!-- <v-navigation-drawer
      v-model="drawer"
      app
    >
      <!--  -->
    </v-navigation-drawer> -->

    <v-navigation-drawer 
      v-model="drawer"
      app
      permanent>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6">
            Application
          </v-list-item-title>
          <v-list-item-subtitle>
            subtext
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list
        dense
        nav
      >
        <v-list-item
          v-for="item in items"
          :key="item.title"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Manim Interactive</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <!--  -->
      <iframe
          id="p5-canvas"
          src="https://openprocessing.org/sketch/1227678/embed/?plusEmbedHash=MWQ5YzJiNzIwNThhYTkwY2NjZWQxMWQyM2Q1NWRhOWU4YWY0NjRkZThhMGQyYTk4MzIwOTlkYTRmZDZhZWNhNjhhMzlhY2RkOTA4N2IxZjRiYmY4ZWE3MTNkZDI3NDI3MTM2YzU0YWRiYTNiNDY5ZjY3ODgyOGY4NzU5ZGYxOGQ1bDJSRklITjJVOGFmMFpyZllrMmM0YXpXbXpKNGJEakFrOTZUTUhaOXZYVjlmNnhkY2pTUFVNVy96NXFLUXh0L3ljaFFWandCUlVTN21kVEhNN0dmZz09&plusEmbedTitle=true"
      ></iframe>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({ drawer: null }),
  }
</script>

<style lang="scss" scoped>
#p5-canvas {
  width: 150vw;
  height: 180vh;
  margin-left: -50px;
  margin-top: -100px;
}
</style>