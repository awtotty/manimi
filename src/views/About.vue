<template>
  <div class="about">
  <v-container class="gray darken-4">
    <v-row>
  <!-- filler -->
  <v-col
    cols="12"
    lg="1"
  >
  
  </v-col>

  <!-- content -->
  <v-col
    cols="12"
    lg="10"
  >
    <v-sheet
      cols="8"
      lg="4"
    >
      <iframe
        id="about-imbed"
        src="https://www.manim.community/"
      >
      </iframe>
    </v-sheet>
  </v-col>

  <!-- filler -->
  <v-col
    cols="12"
    lg="1"
  >
  </v-col>

    </v-row>
  </v-container>
  </div>
</template>



<style lang="scss" scoped>
#about-imbed {
  width: 100%;
  height: 50vw;
  overflow: auto;
}
</style>