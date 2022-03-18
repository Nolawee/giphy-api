<template>
      <logo />
      <search @fetch-gifs="onFetch" @clear-gifs="clearGifs" :bottom-counter="bottomCounter"/>
      <gif-list :columns="columned_gifs"/>
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
      bottomCounter: 0,
      columns: 4,
      columned_gifs: {},
      clear_columned_gifs: {},
      initialScreenSize: window.innerWidth
    }
  },
  created() {
    const vm = this;
    if(vm.initialScreenSize < 600) vm.columns = 1;
    else if(window.innerWidth >= 600 && window.innerWidth < 1000) vm.columns = 2;
    else vm.columns = 4;
  },
  methods: {
    onFetch(result) {
      const vm = this;
      vm.gifs.push(...result);
      vm.generateGrid();
    },
    generateGrid() {
      const vm = this;
      if((Object.keys(vm.columned_gifs).length !== 0) && (vm.columns !== Object.keys(vm.columned_gifs).length)) vm.columned_gifs = {...vm.clear_columned_gifs};
      for(let i = 0; i < vm.columns; i++) {
        vm.columned_gifs[`column${i}`] = []
      }
      for(let i = 0; i < vm.gifs.length; i++) {
        const column = i % vm.columns;
        vm.columned_gifs[`column${column}`].push(vm.gifs[i]);
      }
    },
    clearGifs(result) {
      const vm = this;
      vm.bottomCounter = 0;
      vm.gifs = [...result]
      vm.columned_gifs = {...vm.clear_columned_gifs}

    },
    getScrollEnd() {
      const vm = this;
      window.onscroll = () => {
        if((window.scrollY + window.innerHeight + 1) >= document.documentElement.scrollHeight && vm.gifs.length > 0) {
          vm.bottomCounter++;
        }
      }
    },
    onResize() {
      const vm = this;
      let previousColumnSize = vm.columns;
      window.addEventListener('resize', () => {
          if(window.innerWidth < 600 && (previousColumnSize != 1)) vm.columns = 1;
          else if ((window.innerWidth >= 600 && window.innerWidth < 1000) && (previousColumnSize != 2)) vm.columns = 2;
          else if(window.innerWidth >= 1000 && (previousColumnSize != 4)) vm.columns = 4;
          previousColumnSize = vm.columns;
      })
    }
  },
  mounted() {
    const vm = this;
    vm.getScrollEnd();
    vm.onResize();
  },
  watch: { 
    columns() {
      const vm = this;
      if(vm.gifs.length) vm.generateGrid();

    }
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
    width: 100%;
    height: 100%;
    overflow-y: scroll;
    overflow-x: hidden;
    padding: 1.5vh 2vw;
    margin: 0;
  }
</style>
