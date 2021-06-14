<template>
    <main class="container">
        <div v-if="esito">
            <h1>Nessuna ricerca corrispondente!</h1>
        </div>
        <div v-else class="d-flex flex-wrap">
            <Card v-for="el in arrayFilm" :key="el.id" :item="el"/>
        </div>
    </main>
</template>

<script>
import Card from './Card.vue';

export default {
    name: 'Main',
    props:{
        arrayFilm: Array,
        esito: Boolean
    },
    components: {
        Card
    }
}
</script>

<style lang="scss" scoped>

</style>