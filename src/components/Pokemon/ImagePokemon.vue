<template>
    <div>
        <img v-if="this.id && showImage" :src="this.getImage()" class='imagePokemon' @error="setAltImg" /> 
        <img v-if="!showImage" src="../../assets/images/pokeball.png" class='imagePokemon' />    
    </div>
</template>

<script>
export default {
    data(){
        return {
            showImage : true
        }
    },
    props:{        
        id : Number,
    },
    methods:{
        getImage: function(){
            return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${this.id}.png`
        },
        setAltImg() { 
            this.showImage = !this.showImage
        }            
    }
}
</script>

<style scoped>
    .imagePokemon {
        width: 100px;
        height: 100px;
        position: absolute;
        bottom: 0px;
        right: 0px;
        background-image: url('../../assets/images/pokeball.png');
        background-size: 100%;
        background-repeat: no-repeat;
        background-position: 95% 5%; 
    }        
</style>