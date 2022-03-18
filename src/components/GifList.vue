<template>
  <div id="gifList">
    <div v-for="column in columns" class="column" :key="column">
      <div v-for="gif in column" class="gif" :key="gif.id">
        <img :src="gif.images.original.url">
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'gif-list',
  props: {
    columns: Object
  }
}
</script>

<style scoped>
  #gifList {
    position: relative;
    width: 100%;
    display: flex;
    gap: 10px;
    padding: 0px;
  }

  .column {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .gif {
    position: relative;
    overflow: hidden;
    width: 100%;
}

  img {
    width: 100%;
  }
</style>
