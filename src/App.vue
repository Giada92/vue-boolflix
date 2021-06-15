<template>
  <div id="app">
    <!-- Intestazione della pagina -->
    <Header @sceltaUtente="selezionato"/>
    <!-- /Intestazione della pagina -->

    <!-- Corpo della pagina - lista film -->
    <Main :arrayFilm="risultatiFilm"
          :arraySerie="risultatiSerieTv" />
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
      nome:"",
      apiUrl: "https://api.themoviedb.org/3/search/movie",
      apiUrlTv: "https://api.themoviedb.org/3/search/tv",
      apiKey: "2303a90cfe7bed86062b475952078cbf",
      risultatiFilm: [],
      risultatiSerieTv: []
    }
  },
  components: {
    Header,
    Main
  },
  methods:{
    selezionato:function(newString){
      //console.log(newString);
        this.nome = newString;

        //chiamata film
        axios.
            get(this.apiUrl, {
                params: {
                  api_key: this.apiKey,
                  query: this.nome,
                  language: "it-IT"
                }
              })
              .then((response) => {
                    this.risultatiFilm = response.data.results;
                    //console.log("film", this.risultatiFilm);
              });
        
        //chiamata serieTv
        axios.
            get(this.apiUrlTv, {
                params: {
                  api_key: this.apiKey,
                  query: this.nome,
                  language: "it-IT"
                }
              })
              .then((response) => {
                    //console.log(response);

                    this.risultatiSerieTv = response.data.results;
                    //console.log("serie tv", this.risultatiSerieTv);
              });
        
    }
  }
}
</script>

<style lang="scss">
@import './assets/style/general.scss';
</style>
