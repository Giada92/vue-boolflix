<template>
  <div id="app">
    <!-- Intestazione della pagina -->
    <Header @sceltaUtente="ricerca"
            @cambiatoGenere="genereFilm"/>
    <!-- /Intestazione della pagina -->

    <!-- Corpo della pagina - lista film -->
    <Main :arrayFilm="risultatiFilm"
          :arraySerie="risultatiSerieTv"
          :genereScelto= "genereSelezionato"
          :objectGenri="elencoGeneri" />
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
      apiUrl: "https://api.themoviedb.org/3/search/movie",
      apiUrlTv: "https://api.themoviedb.org/3/search/tv",
      apiKey: "2303a90cfe7bed86062b475952078cbf",
      risultatiFilm: [],
      risultatiSerieTv: [],
      genereSelezionato:"",
      elencoGeneri: []
    }
  },
  components: {
    Header,
    Main
  },
  methods:{
    ricerca:function(newString){
    
    //chiamata film
    axios.
        get(this.apiUrl, {
              params: {
              api_key: this.apiKey,
              query: newString,
              language: "it-IT"
            }
          })
          .then((response) => {
                this.risultatiFilm = response.data.results;
                //console.log(this.risultatiFilm);
      });
        
    //chiamata serieTv
    axios.
        get(this.apiUrlTv, {
            params: {
              api_key: this.apiKey,
              query: newString,
              language: "it-IT"
            }
          })
          .then((response) => {
                this.risultatiSerieTv = response.data.results;
                
      });        
    },
  genereFilm: function(string, object){
    this.genereSelezionato = string;
    this.elencoGeneri = object;
  }
  }
}
</script>

<style lang="scss">
@import './assets/style/general.scss';

::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #f1f1f1; 
}
 
/* Handle */
::-webkit-scrollbar-thumb {
  background: #888; 
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #555; 
}
</style>
