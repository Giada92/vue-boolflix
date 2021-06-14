<template>
  <div class="film text-center">
        <p v-if="titoloFilm()"><span class="fw-bold">Titolo originale:</span> {{ item.original_title }}</p>
        <template v-else>
            <p><span class="fw-bold">Titolo:</span> {{ item.title }}</p>
            <p><span class="fw-bold">Titolo originale:</span> {{ item.original_title }}</p>
        </template>
    
        <!-- if else inline per vedere le bandiere solo sulla lingua italiana e inglese -->
        <div v-if="this.item.original_language == 'en'">
            <img :src="inglese" :alt="item.original_language">
        </div>
        <div v-else-if="this.item.original_language == 'it'">
            <img :src="italiano" :alt="item.original_language">
        </div>
        <div v-else-if="this.item.original_language !== 'it' && this.item.original_language !== 'en'">
            <p>{{ item.original_language }}</p>
        </div>
        <p>{{ item.vote_average }}</p>
  </div>
</template>

<script>
export default {
    name:'Card',
    data(){
        return{
            unicoNome: false,
            inglese: require("../assets/en.png"),
            italiano: require("../assets/it.png")

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
        }
    }
}
</script>

<style lang="scss" scoped>

.film {
    width: calc(100% / 4 - 10px);
    margin: 10px 5px; 
    min-height: 100px;
    background-color: blanchedalmond;

    img {
        width: 12%;
        height: 10%;
    }
}

</style>