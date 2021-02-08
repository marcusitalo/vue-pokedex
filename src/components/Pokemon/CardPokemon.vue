<template>
  <div v-if="this.pokemon" v-bind:style="this.background" class="cardPokemon" @click="showModal()">
      <h2><span>{{this.pokemon.id | formatNumber}}</span>  {{name | capitalize}}</h2>
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
        if(localStorage[this.name]){
            this.pokemon = JSON.parse(localStorage[this.name]) 
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
        name : String,
        url : String,
    },
    filters:{
        capitalize(value){
            return value[0].toUpperCase() + value.slice(1)
        },
        formatNumber(value){
            return "#"+value.toString().padStart(3, '0')
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
                localStorage[this.name] = JSON.stringify(data)
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
    .cardPokemon h2 {
        color: var(--white);
        padding: 15px;
        text-align: left;
    }
    .cardPokemon h2 span {
        border: 1px solid;
        padding: 0px 5px;
        border-radius: 20px;
        font-size: 13px;
        font-weight: 400;
    }
</style>