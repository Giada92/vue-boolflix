<template>
    <main>
        <div v-if="arrayFilm.length == 0" class="bg">
            <img src="../assets/sfondo.jpg" alt="">
            <div class="d-flex justify-content-center">
                <h1>Nessun risultato della ricerca corrente</h1>
            </div>
        </div>
        <!-- In un secondo momento gestire se non ci sono risultati -->
        <div v-else class="container-fluid bg_color">
            <h2>Film Trovati: {{ arrayFilm.length}}</h2>
            <div class="d-flex flex-wrap">
                <Card v-for="el in arrayFilm" :key="el.id" :item="el"/>
            </div>
            <h2>Serie Tv Trovate: {{ arraySerie.length}}</h2>
            <div class="d-flex flex-wrap">
                <CardSerie v-for="element in arraySerie" :key="element.id" :items="element"/>
            </div>
        </div>
    </main>
</template>

<script>
import Card from './Card.vue';
import CardSerie from './CardSerie.vue';


export default {
    name: 'Main',
    props:{
        arrayFilm: Array,
        arraySerie: Array
    },
    components: {
        Card,
        CardSerie
    }
}
</script>

<style lang="scss" scoped>

.bg {
    height: calc(100vh - 80px);
    position: relative;

    img {
        width: 100%;
        object-fit: contain;
        //object-position: 0px 20px;

    }


    h1 {
        position: absolute;
        top:80px;
        font-size: 50px;
        color: white;
    }
}

.bg_color {
    min-height: calc(100vh - 80px);
    background-color: black;
}

h2 {
    text-align: center;
    font-size: 50px;
    padding-top: 15px;
}

</style>