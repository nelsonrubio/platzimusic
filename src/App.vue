<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>PlatziMusic</h1>
    <select v-model="selectCountry">
      <option v-for="country in countries" :key="country.value" :value="country.value">{{ country.name }}</option>
    </select>
    <spinner v-show="loading"></spinner>
    <ul>
      <artist v-for="artist in artists" :key="artist.id" v-bind:artist="artist"></artist>
      <!--<li v-for="artist in artists" :key="artist.id"> {{ artist.name }}</li>-->
    </ul>
  </div>
</template>


<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtistas from './api'

export default {
  name: 'app',
  data () {
    return {
      artists:[],
      countries:[
        { name : 'Argentina', value: 'argentina' },
        { name : 'Colombia', value: 'colombia' },
        { name : 'España', value: 'spain' },
      ],
      selectCountry: 'argentina',
      loading: true
    }
  },
  components:{
    Artist,
    Spinner
  },
  methods:{
    refreshArtist(){
      const self = this
      this.loading = true
      this.artists = []
      getArtistas(this.selectCountry)
      .then(function (artist){
        self.artists = artist
        self.loading = false
      })
    }
  },
  mounted:function(){
    this.refreshArtist()
  },
  watch: {
    selectCountry(){
      this.refreshArtist()
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
