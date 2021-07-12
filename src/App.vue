<template>
  <v-app id="inspire">
    <v-app-bar app>
      <v-toolbar-title>Manim Interactive</v-toolbar-title>

      <v-row class="d-flex flex-row-reverse">
        <v-btn
          class="mx-2"
          fab
          dark
          small 
          color="purple"
          to="/about"
        >
          <v-icon dark>
            mdi-help-box
          </v-icon>
        </v-btn>

        <v-btn
          class="mx-2"
          fab
          dark
          small 
          color="purple"
          to="/"
        >
          <v-icon dark>
            mdi-pencil
          </v-icon>
        </v-btn>

        <v-btn
          class="mx-2"
          fab
          dark
          small 
          color="purple"
        >
          <v-icon dark>
            mdi-download
          </v-icon>
        </v-btn>

      </v-row>
    </v-app-bar>

    <v-main class="grey lighten-3">
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
  export default {
  }
</script>

