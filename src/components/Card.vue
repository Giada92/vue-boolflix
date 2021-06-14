<template>
  <div class="film m-3 text-center">
      <p v-if="titoloFilm()"><span class="fw-bold">Titolo originale:</span> {{ item.original_title }}</p>
      <template v-else>
            <p><span class="fw-bold">Titolo:</span> {{ item.title }}</p>
            <p><span class="fw-bold">Titolo originale:</span> {{ item.original_title }}</p>
      </template>
      <p>{{ item.original_language }}</p>
      <p>{{ item.vote_average }}</p>
  </div>
</template>

<script>
export default {
    name:'Card',
    data(){
        return{
            unicoNome: "",
        }
    },
    props: {
        item: Object
    },
    methods:{
        titoloFilm:function(){
            if(this.item.original_title == this.item.title){
                this.unicoNome = true;
            }else {
                this.unicoNome = false;
            }
        }
    }
}
</script>

<style lang="scss" scoped>

.film {
    width: calc(100% / 5 - 10px);
    min-height: 100px;
    background-color: blanchedalmond;
}

</style>