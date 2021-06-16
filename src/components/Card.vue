<template>
  <div class="film">
      <!-- Recupero la locandina dei film/serie -->
      <img class="poster" :src="imgLocandina()" :alt="item.original_title">

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

            <!-- Elenco del Cast -->
            <span v-if="nomiAttori.length > 0" class="fw-normal">CAST: </span>
            <span class="trama" v-for="nomi, index in nomiAttori" :key="index">{{ nomi }}, </span>
            <!-- /Elenco del Cast -->
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
            ricercaAttori:[],
            nomiAttori:[]
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
            if(this.item.poster_path !== null){
                return this.urlLocandina +  this.item.poster_path;
            } else {
                return require("../assets/no_foto.png"); 
            }
        }
    },
    computed: {
        voto: function(){
            var formula = Math.round(Math.round(this.item.vote_average)/2);
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
            this.idFilm = this.item.id;
            
            //chiamta per recuperare gli attori
            axios.
            get("https://api.themoviedb.org/3/movie/" + this.idFilm +"/credits?api_key=2303a90cfe7bed86062b475952078cbf&language=en-US")
              .then((res) => {
                    this.ricercaAttori = res.data.cast;

                    for(var i=0; i<5; i++){
                        console.log( this.ricercaAttori[i].name);
                        this.nomiAttori.push(this.ricercaAttori[i].name);
                    }
                });
    }     
    
}
</script>

<style lang="scss" scoped>

.film {
    width: calc(100% / 5 - 10px);
    margin: 10px 5px;
    min-height: 80%;
    //background-color: blanchedalmond;
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
    //background-color: rgba(#474646, 0.6);
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