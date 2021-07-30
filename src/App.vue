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
              color="#e07a5f"
              to="/about"
              v-bind="attrs"
              v-on="on"
            >
              <v-icon dark>
                mdi-information
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
              color="#525893" 
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
              color="#81b29a"
              v-bind="attrs"
              v-on="on"
              :loading="downloadInProgress"
              @click="getVid()" 
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
      <router-view 
        ref="appc"
        @download-complete="setDownloadInProgress(false)"
      ></router-view>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      loader: null, 
      downloadInProgress: false,
    }), 
    
    methods: {
      getVid() {
        if (this.downloadInProgress) {
          console.log("Download already in progress"); 
          return; 
        }

        this.setDownloadInProgress(true);

        console.log("Requesting video from server"); 
        try {
          this.$refs.appc.getVid();
        } catch (error) {
          console.log(error); 
          console.log("Something went wrong communicating with the app. You probably aren't in the editor."); 
          this.setDownloadInProgress(false);
        }
      },
      setDownloadInProgress(bool) {
        this.downloadInProgress = bool;
      }
    }, 

  }

</script>

<style lang="scss">
$manim-tan: #ece6e2; 
$manim-blue: #525893; 
$manim-green: #87c2a5; 
$manim-red: #e07a5f; 


#top-bar {
  color: $manim-tan; 
}
</style>