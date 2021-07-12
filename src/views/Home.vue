<template>
  <v-container class="grey lighten-3">
    <v-row>
      <v-col
        cols="12"
        sm="3"
        lg="2"
      >
        <v-sheet
          rounded="lg"
          min-height="268"
          elevation="2"
        >
          <mobjects />
        </v-sheet>
      </v-col>

      <v-col
        cols="12"
        sm="6"
        lg="8"
      >
        <v-sheet
          height="85vh"
          rounded="lg"
        >
          <v-container id="scene-container">
            <scene />
          </v-container>
        </v-sheet>
      </v-col>

      <v-col
        cols="12"
        sm="3"
        lg="2"
      >
        <v-sheet
          rounded="lg"
          min-height="268"
          elevation="2"
        >
          <anims />
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
  import Anims from '../components/Anims.vue'
  import Mobjects from '../components/Mobjects.vue'
  import Scene from "../components/Scene.vue"

  export default {
    components: { 
      Scene, 
      Mobjects,
      Anims  
    }, 

    data: () => ({ 
    }),
  }
</script>


<style lang="scss" scoped>
#scene-container {
  height: 85vh; 
  border-radius: 8px;
  overflow: hidden;
}
</style>