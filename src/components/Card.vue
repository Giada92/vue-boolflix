<template>
  <div class="film">
      <img class="poster" :src="imgLocandina()" alt="">
      <div class="contenitore_text text-center">
            <p v-if="titoloFilm()"><span class="fw-bold">Titolo originale:</span> {{ item.original_title }}</p>
            <template v-else>
                <p><span class="fw-bold">Titolo:</span> {{ item.title }}</p>
                <p><span class="fw-bold">Titolo originale:</span> {{ item.original_title }}</p>
            </template>
        
            <div v-if="bandiera">
                <span>Lingua: </span><img :src="imgBandiera" :alt="item.original_language">
            </div>
            <div v-else>
                <p>{{ item.original_language }}</p>
            </div>
            <p>{{ item.vote_average }}</p>
      </div>
  </div>
</template>

<script>
export default {
    name:'Card',
    data(){
        return{
            unicoNome: false,
            bandiera: false,
            imgBandiera:""
        }
    },
    props: {
        item: Object
    },
    methods:{
        titoloFilm:function(){
            if(this.item.original_title == this.item.title){
                this.unicoNome = true;
            }

            return this.unicoNome;
        },
        imgLocandina: function(){
            return "https://image.tmdb.org/t/p/w342" +  this.item.poster_path;
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
        //height: 100%;
    }
}

.film:hover .contenitore_text{
    display: block;
    width: 100%;
    max-height: 100%;
    position: absolute;
    bottom: 0;
    padding: 10px;
    background-image: linear-gradient(to bottom, rgba(#0000, 0.1), rgba(#0000, 1));
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

</style>