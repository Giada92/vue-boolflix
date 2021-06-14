<template>
    <main class="container">
        <h1>Titolo provvisorio</h1>
        <div class="d-flex justify-content-around">
            <Card />
            <Card />
            <Card />
            <Card />
            <Card />
        </div>
    </main>
</template>

<script>
import Card from './Card.vue';

export default {
    name: 'Main',
    components: {
        Card
    },
}
</script>

<style lang="scss" scoped>

</style>