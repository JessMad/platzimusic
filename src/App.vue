<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul 
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbind")
</template>

<script>
import {getArtists, getCountries} from './api'
import Artist from './components/Artist'
import spinner from './components/spinner.vue'
export default {
  name: 'app',
  data () {
    return {
      artists:[],
      countries:[
        { name:'Argentina', value: "argentina"  },
        { name:'México', value:"mexico" },
        { name: 'España', value: 'spain'}
      ],
      selectedCountry: 'mexico',
      loading : true
    }
  },
  components: {
    Artist,
    spinner
  },
  methods:{
    refreshArtists(){
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.loading = false
          self.artists = artists
        })
    }
  },
  mounted: function(){
    this.refreshArtists()
  },
  watch: {
      selectedCountry: function(){
        this.refreshArtists()          
      }
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50 
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
