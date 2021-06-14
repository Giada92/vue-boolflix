<template>
  <div id="app">
    <!-- Intestazione della pagina -->
    <Header @sceltaFilm="filmScelto"/>
    <!-- /Intestazione della pagina -->

    <!-- Corpo della pagina - lista film -->
    <Main :arrayRicerca="risultatiRicerca"
          :esito="notResult" />
    <!-- Corpo della pagina - lista film -->


  </div>
</template>

<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue';

import axios from 'axios';


export default {
  name: 'App',
  data(){
    return{
      nomeFilm:"",
      apiUrl: "https://api.themoviedb.org/3/search/movie",
      apiKey: "2303a90cfe7bed86062b475952078cbf",
      risultatiRicerca: [],
      notResult: false
    }
  },
  components: {
    Header,
    Main
  },
  methods:{
    filmScelto:function(newString){
      //console.log(newString);
        this.nomeFilm = newString;

        axios.
            get(this.apiUrl, {
                params: {
                  api_key: this.apiKey,
                  query: this.nomeFilm,
                  language: "it-IT"
                }
              })
              .then((response) => {
                this.risultatiRicerca = response.data.results;
                //console.log(this.risultatiRicerca);

                if(this.risultatiRicerca.length == 0){
                  this.notResult = true;
                } else {
                  this.notResult = false;
                }

        })
    }
  }
}
</script>

<style lang="scss">
@import './assets/style/general.scss';
</style>
