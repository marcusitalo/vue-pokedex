<template>
    <img v-if="this.id" :src="this.getImage()" class='imagePokemon' />      
</template>

<script>
export default {
    props:{        
        id : Number,
    },
    methods:{
        getImage: function(){
            return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${this.id}.png`
        }
    }
}
</script>

<style scoped>
    .imagePokemon {
        width: 100px;
        position: absolute;
        bottom: 0px;
        right: 0px;
        background-image: url('../../assets/images/pokeball.png');
        background-size: 100%;
        background-repeat: no-repeat;
        background-position: 95% 5%; 
    }        
</style>