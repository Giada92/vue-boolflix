<template>
  <div class="film">
      <!-- Recupero la locandina dei film/serie -->
      <img class="poster" :src="imgLocandina()" :alt="ricercaTitoloOriginale()">

      <div class="info text-center">
            <!-- Stampo  i titoli -->
            <p v-if="nomeUnico()"><span class="fw-bold">Titolo originale:</span> {{ ricercaTitoloOriginale() }}</p>
            <template v-else>
                <p><span class="fw-bold">Titolo:</span> {{ ricercaTitolo() }}</p>
                <p><span class="fw-bold">Titolo originale:</span> {{ ricercaTitoloOriginale() }}</p>
                <p>{{ item.id }}</p>
            </template>
            <!-- /Stampo  i titoli -->

            <!-- Assegno le bandiere in base alla lingua -->
            <div v-if="bandiera">
                <span>Lingua: </span><img :src="imgBandiera" :alt="item.original_language">
            </div>
            <div v-else>
                <p>{{ item.original_language }}</p>
            </div>
            <!-- /Assegno le bandiere in base alla lingua -->

            <!-- Assegno le stielle per il voto -->
            <p class="voto">Voto: 
                <i v-for="i in 5" :key="i" :class="i <= voto ? 'fas fa-star':'far fa-star'"></i>
            </p>
            <!-- /Assegno le stielle per il voto -->

            <!-- Trama -->
            <p v-if="item.overview.length > 0" class="trama"><span class="fw-bold">Trama:</span> {{ item.overview }} </p>
            <!-- /Trama -->

            <!-- Elenco del Cast dei film -->
            <span v-if="nomiAttoriFilm.length > 0" class="fw-normal">CAST: </span>
            <span class="trama" v-for="nomi, index in nomiAttoriFilm" :key="index">{{ nomi }}, </span><br>
            <!-- /Elenco del Cast dei film -->

            <!-- Elenco del Cast delle serie tv -->
            <span v-if="nomiAttoriSerie.length > 0" class="fw-normal">CAST: </span>
            <span class="trama" v-for="nomi, index in nomiAttoriSerie" :key="index">{{ nomi }}, </span><br>
            <!-- /Elenco del Cast delle serie tv -->

            <!-- Elenco Genre del film -->
            <span v-if="stampaGenere.length > 0" class="fw-normal">Genere: </span>
            <span class="trama" v-for="genere, index in stampaGenere" :key="index">{{ genere }}, </span>
            <!-- /Elenco Genre del film -->
      </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
    name:'Card',
    data(){
        return{
            unicoNome: false,
            bandiera: false,
            imgBandiera:"",
            urlLocandina: "https://image.tmdb.org/t/p/w342",
            apiKey: "2303a90cfe7bed86062b475952078cbf",
            idFilm: [],
            idSerie: [],
            ricercaAttoriFilm:[],
            ricercaAttoriSerie:[],
            nomiAttoriFilm:[],
            nomiAttoriSerie:[],
            genereFilm:[],
            stampaGenere:[]
        }
    },
    props: {
        item: Object
    },
    methods:{
        ricercaTitoloOriginale: function(){
            if(this.item.original_title){
                return this.item.original_title;
            }else {
                return this.item.original_name;
            }
        },
        ricercaTitolo: function(){
            if(this.item.title){
                return this.item.title;
            }else {
                return this.item.name;
            }
        },
        nomeUnico:function(){
            if(this.item.original_title == this.item.title){
                this.unicoNome = true;
            }
            return this.unicoNome;
        },
        imgLocandina: function(){
            if(this.item.poster_path != null){
                return this.urlLocandina +  this.item.poster_path;
            } else {
                return require("../assets/no_foto.png"); 
            }
        }
    },
    computed: {
        voto: function(){
            var formula = Math.ceil(this.item.vote_average / 2);
            return formula;
        }
    },
    created(){
        var linguaOriginale = this.item.original_language;
        if(linguaOriginale == 'en' || linguaOriginale == 'it') {
            if(linguaOriginale == 'en') {
                this.imgBandiera = require("../assets/en.png");
            } else if(linguaOriginale == 'it') {
                this.imgBandiera = require("../assets/it.png");
            }

            this.bandiera = true;

        }
    },
    mounted(){
        
        if(this.item.original_title){
            this.idFilm.push(this.item.id);
        }else {
            this.idSerie.push(this.item.id);
        }
        
        //chiamta per recuperare gli attori dei film
        /* FUNZIONA IN PARTE */
        axios.
        get("https://api.themoviedb.org/3/movie/" + this.idFilm +"/credits?api_key=2303a90cfe7bed86062b475952078cbf&language=en-US")
            .then((res) => {

                for(var i=0; i<5; i++){
                    if(res.data.cast[i] != undefined){
                        console.log(res.data.cast);
                        this.ricercaAttoriFilm = res.data.cast;
                        //console.log( this.ricercaAttori[i].name);
                        this.nomiAttoriFilm.push(this.ricercaAttoriFilm[i].name);
                    }
                }
            });

        //chiamta per recuperare gli attori delle serie tv
        /* FUNZIONA IN PARTE */
        axios.
        get("https://api.themoviedb.org/3/tv/" + this.idSerie +"/credits?api_key=2303a90cfe7bed86062b475952078cbf&language=en-US")
            .then((res) => {

                for(var i=0; i<5; i++){
                    if(res.data.cast[i] != undefined){
                        this.ricercaAttoriSerie = res.data.cast;
                        //console.log( this.ricercaAttori[i].name);
                        this.nomiAttoriSerie.push(this.ricercaAttoriSerie[i].name);
                    }
                }
            });

        //chiamata per recuperare i generi del film
        /* axios.
        get("https://api.themoviedb.org/3/movie/" + this.idFilm +"?api_key=2303a90cfe7bed86062b475952078cbf&language=en-US")
            .then((res) => {
                this.ricercaAttoriFilm = res.data.genres;

                for(var i=0; i<this.ricercaAttoriFilm.length; i++){
                    this.stampaGenere.push(this.ricercaAttoriFilm[i].name);
                }
            }); */
    }     
    
}
</script>

<style lang="scss" scoped>

.film {
    width: calc(100% / 5 - 10px);
    margin: 10px 5px;
    min-height: 80%;
    position: relative;

    .poster {
        width: 100%;
        cursor: pointer;
        height: 100%;
    }
}

.film:hover .info{
    display: block;
    width: 100%;
    max-height: 100%;
    position: absolute;
    overflow-y: auto;
    bottom: 0;
    padding: 10px;
    background-image: linear-gradient(to bottom, rgba(#0000, 0.6), rgba(#0000, 2));
}

.info {
    display: none;
    
    img {
        width: 10%;
        height: 10%;
    }
}

span {
    font-size: 20px;
    color: red;
}

p {
    color: white;
    font-size: 20px;
}

.voto {
    margin-top: 10px;
}

.trama {
    text-align: justify;
    font-size: 14px;
}
</style>