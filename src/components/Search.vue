<template>
    <div class="input-group">
      <div>
        <input placeholder="Search..." v-model="keyword" @keyup.enter="onEnter">
        <button @click="onClick" class="search-button">
          <!-- Grabbed this SVG from Giphy's website -->
          <svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <defs>
              <path d="M11.5482521,20.4090671 L4.24727698,28.2009189 C3.68084207,28.8054377 2.73159653,28.8363108 2.12707771,28.2698759 C1.5225589,27.703441 1.4916858,26.7541954 2.0581207,26.1496766 L9.40599838,18.3077689 C7.95982241,16.4371424 7.0978836,14.0789715 7.0978836,11.5181818 C7.0978836,5.44914339 11.9392549,0.518181818 17.9252787,0.518181818 C23.9113026,0.518181818 28.7526738,5.44914339 28.7526738,11.5181818 C28.7526738,17.5872202 23.9113026,22.5181818 17.9252787,22.5181818 C15.539851,22.5181818 13.3361963,21.7351359 11.5482521,20.4090671 Z M17.9252787,19.5181818 C22.242011,19.5181818 25.7526738,15.9425536 25.7526738,11.5181818 C25.7526738,7.09381 22.242011,3.51818182 17.9252787,3.51818182 C13.6085464,3.51818182 10.0978836,7.09381 10.0978836,11.5181818 C10.0978836,15.9425536 13.6085464,19.5181818 17.9252787,19.5181818 Z" id="path-1"></path>
            </defs>
            <g id="search" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
              <g id="icons/search">
                <mask id="mask-2" fill="white">
                  <use xlink:href="#path-1"></use>
                </mask>
                <use id="Mask" fill="#FFFFFF" fill-rule="nonzero" xlink:href="#path-1"></use>
                <g id="colors/gradients/_master" mask="url(#mask-2)">
                  <g transform="translate(0.250000, 0.250000)" id="colors/gradients/white">
                    <g></g>
                  </g>
                </g>
              </g>
            </g>
          </svg>
        </button>
      </div>
    </div>
    <ul v-if="searchHistory.length" id="previous-searches">
      <li class="showing-results" style="display:inline; list-style-type: none;">Previous Searches:  </li>
      <li style="display:inline; list-style-type: none;" v-for="search in searchHistory" :key="search">
        <span @click="onClickPreviousSearch(search)" class="previous-search-items">{{search}}</span>
      </li>
    </ul>
    
    <p v-if="searchHistory.length > 0 && showResult" class="showing-results">Showing results for: <b style="margin-left: 10px;">{{searchHistory[searchHistory.length - 1]}}</b></p>
</template>

<script>

export default {
  name: 'Search',
  props: {
    bottomCounter: Number
  },
  data() {
    return {
      keyword: '',
      limit: 30,
      timeout: null,
      showResult: false,
      searchHistory: [],
      finalPage: 0
    }
  },
  methods: {
    onEnter() {
      const vm = this;
      clearTimeout(vm.timeout);
      vm.timeout = setTimeout(() => {
        vm.onClick()
      }, 500)
    },
    onClick() {
      const vm = this;
      if(vm.keyword.length) {
        vm.$emit('clear-gifs', []);
        vm.showResult = false;
        vm.search();
        vm.searchHistory.push(vm.keyword);
      } else {
        vm.$emit('clear-gifs', []);
        vm.showResult = false;
      }
    },
    onClickPreviousSearch(search) {
      const vm = this;
      vm.searchHistory = vm.searchHistory.filter(e => e !== search);
      vm.keyword = search;
      vm.$emit('clear-gifs', []);
      vm.showResult = false;
      vm.search()
      vm.searchHistory.push(vm.keyword);
    },
    search() {
      const vm = this;
      let url = `${process.env.VUE_APP_GIPHY_API_URL}/gifs/search?api_key=${process.env.VUE_APP_GIPHY_API_KEY}&q=${vm.keyword}&limit=${vm.limit}`
      if(vm.bottomCounter > 0) {
        let offset = vm.limit * vm.bottomCounter;
        if(offset > vm.finalPage) return; // stop scrolling
        url = url+ `&offset=${offset}`;
      }
      fetch(url)
      .then(response => response.json())
      .then(result => {
        vm.finalPage = result.pagination.total_count - vm.limit;
        vm.$emit('fetch-gifs', result.data)
      }).then(() => {
        vm.showResult = true;
      })
    }
  },
  watch: { 
    bottomCounter() {
      const vm = this;
      vm.search();
    }
  }
}

</script>

<style scoped>
  #previous-searches {
    padding: 0px;
    margin-bottom: 30px;
  }

  .showing-results {
    color: #B2B2B2;
  }

  .previous-search-items {
    margin: 0px 10px;
    font-size: 13px;
    color: #01c6f9;
    text-decoration: underline;
  }

  .previous-search-items:hover{
    cursor: pointer;
  }

  .search-button {
    width: 52px;
    height: 52px;
    outline: 0;
    border: none;
    background-image: linear-gradient(to top right, #9b33fc, #ec5e80);
  }

  .search-button:hover {
    cursor: pointer;
  }
  .input-group {
    display: inline-block;
    width: 100%;
  }

  input {
    padding: 10px 16px;
    border-radius: 0px;
    font-size: 18px;
    outline: 0;
    height: 53px;
    width: 96%;
    border: 2px solid #5f5f5f;
    vertical-align: bottom;
  }

  input:focus {
    border-color: #1c1ca8;
  }

  .loading {
    background-color: black;
    height: 100px;
  }

  @media only screen and (min-width: 2669px) {
    input {
      width: 98%;
    }
  }

  @media only screen (min-width: 1900px) and (max-width: 2668.98px) {
    input {
      width: 97%;
    }
  }

  @media only screen and (max-width: 1300px) {
    input {
      width: 94%;
    }
  }

  @media only screen and (max-width: 982px) {
    input {
      width: 92%;
    }
  }

  @media only screen and (max-width: 858px) {
    input {
      width: 91%;
    }
  }

  @media only screen and (max-width: 765px) {
    input {
      width: 90%;
    }
  }

  @media only screen and (max-width: 693px) {
    input {
      width: 89%;
    }
  }

  @media only screen and (max-width: 635px) {
    input {
      width: 88%;
    }
  }

  @media only screen and (max-width: 587px) {
    input {
      width: 89%;
    }
  }
  @media only screen and (max-width: 364px) {
    input {
      width: 80%;
    }
  }

</style>