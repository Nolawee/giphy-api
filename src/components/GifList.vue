<template>
  <div id="gifList">
    <gif v-for="gif in gifs" :key="gif.id" :gif="gif"/>
  </div>
</template>

<script>
import Gif from "@/components/Gif";

export default {
  name: 'gif-list',
  components: {Gif},
  props: {
      gifs: Array
  }
}
</script>

<style scoped>
  #gifList {
    margin-top: 30px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }

  @media (max-aspect-ratio: 1/1) and (max-width: 480px) {
    #gifList {
      flex-direction: row;
    }
  }
</style>
