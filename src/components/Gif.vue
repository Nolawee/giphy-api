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
/* I used https://css-tricks.com/adaptive-photo-layout-with-flexbox/ as a guide to style the gifs */

  .gif {
    height: 17vh;
    flex-grow: 1;
  }
  img {
    max-height: 100%;
    min-width: 100%;
    object-fit: cover;
    vertical-align: bottom;
  }

  @media (max-aspect-ratio: 1/1) {
    .gif {
      height: 30vh;
    }
  }

  @media (max-height: 480px) {
    .gif {
      height: 80vh;
    }
  }

  @media (max-aspect-ratio: 1/1) and (max-width: 480px) {
    .gif {
      height: auto;
      width: 100%;
    }

    img {
      width: 100%;
      max-height: 75vh;
      min-width: 0;
    }
  }
</style>
