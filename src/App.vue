<template>
  <div id="app">
    <!-- Intestazione della pagina -->
    <Header @sceltaFilm="filmScelto"/>
    <!-- /Intestazione della pagina -->

    <!-- Corpo della pagina - lista film -->
    <Main />
    <!-- Corpo della pagina - lista film -->


  </div>
</template>

<script>
import Header from './components/Header.vue';
import Main from './components/Main.vue'


export default {
  name: 'App',
  data(){
    return{
      nomeFilm:""
    }
  },
  components: {
    Header,
    Main
  },
  methods:{
    filmScelto:function(newString){
      console.log(newString);
        this.nomeFilm = newString;
    }
  }
}
</script>

<style lang="scss">
@import './assets/style/general.scss';
</style>
