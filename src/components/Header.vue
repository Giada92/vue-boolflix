<template>
    <header class="d-flex justify-content-between align-items-center px-3">
        <div>
            <a href="#">
                <img src="../assets/my_logo.png" alt="Logo Personalizzato">
            </a>
        </div>
        <div class="container-select d-flex justify-content-between">
            <select v-model="selezionaGenere" @change="$emit('cambiatoGenere', selezionaGenere, objetGeneri)">
                <option value="">Scegli il genere del Film</option>
                <option v-for="genere, index in generiFilm" :key="index">{{genere}}</option>
            </select>
            <form>
                <input class="mx-3 py-0" type="text" v-model.trim="ricercaUtente" placeholder="Search...">
                <button @click.prevent="$emit('sceltaUtente', ricercaUtente)"
                    class="btn btn-dark py-1" type="submit"><i class="fas fa-search"></i>
                </button>
            </form>
        </div>
    </header>
</template>

<script>
import axios from 'axios';

export default {
    name: 'Header',
    data(){
        return{
            ricercaUtente: "",
            apiUrl: "https://api.themoviedb.org/3/genre/movie/list",
            apiKey: "2303a90cfe7bed86062b475952078cbf",
            generiFilm:[],
            selezionaGenere: "",
            objetGeneri: []
        }
    },
    mounted(){    
    //chiamata generi dei film
    axios.
        get(this.apiUrl, {
              params: {
              api_key: this.apiKey,
              language: "it-IT"
            }
          })
          .then((res) => {
              let prova = res.data.genres;
              prova.forEach(
                  (element)=>{
                      this.generiFilm.push(element.name);
                      this.objetGeneri.push(element);
                      //console.log(this.objetGeneri);

                  });
      });
    }
}
</script>

<style lang="scss" scoped>

header {
    width: 100%;
    height: 80px;
    background: black;

    img {
        width: 200px;
    }
}


</style>