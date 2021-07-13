<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title>
        Manim Interactive
      </v-toolbar-title>

      <v-row class="d-flex flex-row-reverse">
        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="mx-2"
              fab
              dark
              small 
              color="#525893" 
              to="/about"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon dark>
                mdi-help-box
              </v-icon>
            </v-btn>
          </template>
          <span>About</span>
        </v-tooltip>

        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="mx-2"
              fab
              dark
              small 
              color="#81b29a"
              to="/"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon dark>
                mdi-pencil
              </v-icon>
            </v-btn>
          </template>
          <span>Create</span>
        </v-tooltip>

        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              class="mx-2"
              fab
              dark
              small 
              color="#e07a5f"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon dark>
                mdi-download
              </v-icon>
            </v-btn>
          </template>
          <span>Download</span>
        </v-tooltip>

      </v-row>
    </v-app-bar>

    <v-main> 
      <router-view></router-view>
    </v-main>
  </v-app>
</template>

<script>
  export default {
  }
</script>

<style lang="scss">
$manim-tan: #ece6e2; 
$manim-blue: #525893; 
$manim-green: #87c2a5; 
$manim-red: #e07a5f; 

// * {
//   color: $manim-blue; 
// }

#top-bar {
  color: $manim-tan; 
}
</style>