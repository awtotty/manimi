<template>
  <scene />
</template>


<script>
  import Scene from '../components/Scene.vue'

  export default {
    name: 'Home',

    components: {
        Scene
    },
  }
</script>


<style lang="scss" scoped>
#p5-canvas {
  width: 105vw;
  height: 105vh;
  margin-left: -50px;
  margin-top: -100px;
  overflow: hidden;
}
</style>