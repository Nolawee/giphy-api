<template>
    <div class="gif">
      <img :src="gif.images.original.url">
    </div>
</template>

<script>
export default {
  name: 'Gif',
  props: {
    gif: Object
  }
}
</script>

<style scoped>

  .gif {
    position: relative;
    overflow: hidden;
    width: 100%;
  }
  img {
    width: 100%;
  }
</style>
