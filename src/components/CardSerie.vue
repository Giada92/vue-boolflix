<template>
    <div class="film text-center">
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
        <p><span class="fw-bold">Voto:</span> {{ items.vote_average }}</p>
    </div>
  
</template>

<script>
export default {
    name:"CardSerie",
    data(){
        return{
            unicoNome: false,
            bandiera: false,
            imgBandiera:""
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
    min-height: 100px;
    background-color: blanchedalmond;

    img {
        width: 12%;
        height: 10%;
    }
}

</style>