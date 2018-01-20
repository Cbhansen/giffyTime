<template>
  <div id="app" class="container">
    <h1>{{ title }}</h1>
    <form v-on:submit.prevent="search()"> 
      <label class="form-group" for="search">
        <input class="form-control" type="text" id="search" placeholder="Search gif" v-model="searchVal" >
        <hr>
        <button class="btn btn-lg btn-outline-warning">Search!</button>
      </label>
    </form>

    <div id="giph-container">
      <h4>Giff results</h4>
      <ul v-for="gif in gifs" :key="gif.id">
        <a target="_blank" :href="gif.url"><img class="gif-img" :src="gif.images.fixed_height.url" :alt="gif.title"></a>
      </ul>
    </div>
  </div>
</template>

<script>
const API_KEY = 'O0ssPUC9iM4pSTPZYL9YxruWetsNgH0n'
export default {
  name: 'app',
  data () {
    return {
      title: 'Giphy Time!',
      gifs: [],
      searchVal: '',
    }
  },
  methods: {
    search() {
      let vm = this;
      fetch('http://api.giphy.com/v1/gifs/search?q='+this.searchVal+'&api_key='+API_KEY+'&limit=10')
        .then(function(response) {
          return response.json();
        })
        .then(function(response) {
          console.log(response.data);
          vm.gifs = response.data;
        })
        .catch(function(error) {
          console.log('An error occured:', error);
        })
    },
    trending() {
      let vm = this;
      fetch('http://api.giphy.com/v1/gifs/trending?q=&api_key='+API_KEY+'&limit=10')
        .then(function(response) {
          return response.json();
        })
        .then(function(response) {
          console.log(response.data);
          vm.gifs = response.data;
        })
        .catch(function(error) {
          console.log('An error occured:', error)
        })
    }
  },
  created() {
    this.trending();
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background: #000;
}

h1 {
  color: palevioletred;
}
h4 {
  color: purple;
}

.gif-img {
  border: 3px solid orangered;
}
</style>
