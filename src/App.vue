<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>MusicFM</h1>
    <select v-model="selectedCountry">
      <option v-for="country in countries" :value="country.value"> {{country.name}} </option>
    </select>
    <loader v-show="loading"></loader>
    <ul>
      <artist v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid"> 
      </artist>
    </ul>
  </div>
</template>

<script>
import Artist from './components/Artist.vue'
import Loader from './components/Loader.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries:[
        { name: 'Spain', value: 'spain' },
        { name: 'Cyprus', value: 'cyprus' },
        { name: 'USA', value: 'united states of america' },
        { name: 'Venezuela', value: 'venezuela' },
      ],
      selectedCountry: 'spain',
      loading: true
    }
  },
  components: {
    Artist,
    Loader
  },
  methods: {
    refreshArtists () {
      this.loading = true
      this.artists = []
       getArtists(this.selectedCountry)
      .then((artists) =>  {
        this.artists = artists
        this.loading = false
      })
    }
  },
  mounted () {
    this.refreshArtists()
  },
  watch: {
    selectedCountry () {
      this.refreshArtists()
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
