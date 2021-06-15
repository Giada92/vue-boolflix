<template>
    <header class="d-flex justify-content-between align-items-center px-3">
        <div>
            <img src="../assets/my_logo.png" alt="Logo Personalizzato">
        </div>
        <div>
            <form>
                <input class="mx-3" type="text" v-model.trim="ricercaUtente" placeholder="Search...">
                <button @click.prevent="$emit('sceltaUtente', ricercaUtente)"
                    class="btn btn-light" type="submit">Cerca
                </button>
            </form>
        </div>
    </header>
</template>

<script>
export default {
    name: 'Header',
    data(){
        return{
            ricercaUtente: ""
        }
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