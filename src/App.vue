<template>
    <logo />
    <search @fetch-gifs="onFetch" @clear-gifs="clearGifs" :bottom-counter="bottomCounter"/>
    <gif-list :gifs="gifs"/>  
</template>

<script>
import Logo from "@/components/Logo"
import Search from "@/components/Search"
import GifList from "@/components/GifList"

export default {
  name: 'App',
  components: {
    Search,
    GifList,
    Logo
  },
  data() {
    return {
      gifs: [],
      bottomCounter: 0
    }
  },
  methods: {
    onFetch(result) {
      const vm = this;
      vm.gifs.push(...result);
    },
    clearGifs(result) {
      const vm = this;
      vm.bottomCounter = 0;
      vm.gifs = [...result]
    },
    getScrollEnd() {
      const vm = this;
      window.onscroll = () => {
        if((window.scrollY + window.innerHeight + 1) >= document.documentElement.scrollHeight && vm.gifs.length > 0) {
          vm.bottomCounter++;
        }
      }
    }
  },
  
  mounted() {
    const vm = this;
    vm.getScrollEnd();
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300;400;700&display=swap');
</style>

<style>
  * {
    box-sizing: border-box;
    font-family: 'Lato', sans-serif;
  }
  body {
    background-color: #000000;
    padding: 50px;
  }
</style>
