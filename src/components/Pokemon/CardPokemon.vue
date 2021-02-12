<template>
  <div v-if="this.pokemon" v-bind:style="this.background" class="cardPokemon" @click="showModal()">
      <pre>{{this.pokemon.name | replaceCaracter}}</pre>
      <div class="labelNumber"><span>{{this.pokemon.id | formatNumber}}</span> </div>
      <ImagePokemon :id="this.pokemon.id" />
      <TypePokemon :types="this.pokemon.types" @applyBackgroundColor="setBackgroundColor($event)"/>
  </div>
</template>

<script>
import axios from 'axios'
import TypePokemon from './TypePokemon'
import ImagePokemon from './ImagePokemon'

export default {
    async created(){
        if(localStorage[this.slug]){
            this.pokemon = JSON.parse(localStorage[this.slug]) 
            return;   
        }
        else{
            this.getInApi()
        }
    },
    components: {
      TypePokemon,
      ImagePokemon
    },
    data(){
        return {
            pokemon : {},
            background: {
                backgroundColor : 'transparent'
            }
        }        
    },
    props:{        
        number : Number,
        slug : String,
        url : String,
    },
    filters:{
        formatNumber(value){
            return "#"+(""+value).padStart(3, '0')
        },
        replaceCaracter(value){             
            return value ? value.replaceAll('-', '\n') : 'N/D'
        }           
    },
    methods:{
        getInApi: async function(){
            await axios.get(this.url).then(res => {
                const data = {
                    id : res.data.id,
                    name : res.data.name,            
                    types : this.clearTypes(res.data.types),
                    abilities: this.clearAbilities(res.data.abilities),              
                    moves :  this.clearMoves(res.data.moves),             
                }
                this.pokemon = data
                localStorage[this.slug] = JSON.stringify(data)
            }).catch();
        },
        setBackgroundColor(data) {
            this.background = { backgroundColor : data.color || '#777' }
        },
        showModal: function(){
            this.$emit("showDetails",{ background : this.background.backgroundColor, pokemon : this.pokemon })
        },
        clearTypes: function(types){
            let typesClean = types.map(function(type) {
                if(type.slot)
                    delete type.slot
                if(type.type.url)
                    delete type.type.url
                return type
            })
            return typesClean;
        },
        clearMoves: function(moves){
            let movesClean = moves.map(function(move) {
                if(move.version_group_details)
                    delete move.version_group_details
                if(move.move.url)
                    delete move.move.url
                return move
            })
            return movesClean;
        },
        clearAbilities: function(abilities){
            let abilitiesClean = abilities.map(function(ability) {
                if(ability.is_hidden)
                    delete ability.is_hidden
                if(ability.slot)
                    delete ability.slot
                if(ability.is_hidden)
                    delete ability.is_hidden
                if(ability.ability.url)
                    delete ability.ability.url
                return ability
            })
            return abilitiesClean;
        }              
    }
}
</script>

<style scoped>
    @import '../../assets/css/keyframes/showDelayCard.css';

    .cardPokemon{
        width: 250px;
        height: 80px;
        font-size: 9px;
        margin: 10px;
        border-radius: 20px;
        position:relative; 
         will-change: animation;
        animation: showDelayCard 1s ease-in-out;        
    }
    .cardPokemon:hover{
        transform: scale(1.05);
        cursor: pointer;
    }
    .cardPokemon pre {
        color: var(--white);
        padding: 5px 70px;
        text-align: left;
        text-transform: capitalize;
        font-size: 13px;
        font-weight: bold;
    }
    .labelNumber span {
       border: 1px solid;
        padding: 1px 6px;
        border-radius: 20px;
        font-size: 9px;
        font-weight: 400;
        color: var(--white);
    }
    .labelNumber {
        position: absolute;
        top: 8px;   
        left: 15px;
    }
</style>