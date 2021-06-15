<template>
    <div class="film text-center">
        <img class="poster" :src="imgLocandina()" :alt="items.original_title">
        <div class="contenitore_text text-center">
            <p v-if="titoloSerie()"><span class="fw-bold">Titolo originale:</span> {{ items.original_name }}</p>
            <template v-else>
                <p><span class="fw-bold">Titolo:</span> {{ items.name }}</p>
                <p><span class="fw-bold">Titolo:</span> {{ items.original_name }}</p>
            </template>
            <div v-if="bandiera">
                <img :src="imgBandiera" :alt="items.original_language">
            </div>
            <div v-else>
                <p>{{ items.original_language }}</p>
            </div>
            <p class="voto">Voto: 
                <i v-for="i in 5" :key="i" :class="i <= voto ? 'fas fa-star':'far fa-star'"></i>
            </p>
        </div>
    </div>
  
</template>

<script>
export default {
    name:"CardSerie",
    data(){
        return{
            unicoNome: false,
            bandiera: false,
            imgBandiera:"",
            urlLocandina: "https://image.tmdb.org/t/p/w342"
        }
    },
    props: {
        items: Object
    },
    methods:{
        titoloSerie:function(){
            if(this.items.original_name == this.items.name){
                this.unicoNome = true;
            }

            return this.unicoNome;
        },
        imgLocandina: function(){
            if(this.items.poster_path !== null){
                return this.urlLocandina +  this.items.poster_path;
            } else {
                //console.log(this.items);
                return require("../assets/no_foto.png"); 
            }
        }
    },
    computed: {
        voto: function(){
            //console.log(this.item.vote_average);
            var formula = Math.round(Math.round(this.items.vote_average)/2);
            //console.log(formula);
            return formula;
        }
    },        

    created(){
        var linguaOriginale = this.items.original_language;
        if(linguaOriginale == 'en' || linguaOriginale == 'it') {
            if(linguaOriginale == 'en') {
                this.imgBandiera = require("../assets/en.png");
            } else if(linguaOriginale == 'it') {
                this.imgBandiera = require("../assets/it.png");
            }

            this.bandiera = true;
        }
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

.film:hover .contenitore_text{
    display: block;
    width: 100%;
    max-height: 100%;
    position: absolute;
    bottom: 0;
    padding: 10px;
    background-image: linear-gradient(to bottom, rgba(#0000, 0.6), rgba(#0000, 2));
    //background-color: rgba(#474646, 0.6);
}

.contenitore_text {
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
</style>