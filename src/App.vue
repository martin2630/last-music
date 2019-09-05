<template>
  <div id="app">
    <img src="https://martin2630.github.io/last-music/dist/logo.png">
    <h1> Platzi music</h1>
    <h3>Selecciona un país</h3>
    <select name="" id="" v-model="selectedCountry">
      <option v-for="country in countries" v-bind:value="country.value">{{country.name}}</option>
    </select>
    <spinner v-show="loading"></spinner>
    <ul>
      <artist v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid"></artist>
        <!-- <li >
          <a href="#">{{artist.name}}</a>
        </li> -->
    </ul>
  </div>
</template>

<script>
import getArtists from '../api';
import Artist from './components/artist.vue';
import Spinner from './components/spinner.vue';

export default {
  name: 'app',
  data () {
    return {
      loading: false,
      selectedCountry: 'mexico',
      artists: [],
      countries: [
        {
          name: "México",
          value: "mexico"
        },
        {
          name: "Colombia",
          value: "colombia"
        },
        {
          name: "España",
          value: "spain"
        }

      ]
    }
  },
  components:{
    Artist,
    Spinner
  },
  mounted: function() {
    this.refreshArtists();
  },
  methods: {
    refreshArtists: function(){
      this.loading = true;
      const self = this;
      getArtists(this.selectedCountry)
      .then( function(artists){
        self.loading = false;
        self.artists = artists;
      })
    }
  },
  watch: {
    selectedCountry: function(){
      this.refreshArtists();
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
