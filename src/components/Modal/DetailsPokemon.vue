<template>
  <div class="modal-backdrop" >
    <div class="modal" :style="{'--close-color': details.background }">
      <button type="button" class="btn-close" @click="close" > x </button>         
      <section class="modal-body">
        <div class="header-details" v-bind:style="{background: details.background }">
          <p>{{ details.pokemon.id | formatNumber}}</p>
          <h1>{{ details.pokemon.name}}</h1>
          <div class='vieImage'>
            <ImagePokemon :id="details.pokemon.id" class="imageDetails" />          
          </div>            
        </div>    
        <div class="body-details">
          <TypePokemon :types="details.pokemon.types" class="typesDetails"  />
          <AbilitiesPokemon :abilities="details.pokemon.abilities" class="abilitiesDetails"/>
          <MovesPokemon :moves="details.pokemon.moves" :scrollColor="details.background" class="movesDetails"/>
        </div>
      </section>       
    </div>
  </div>
</template>

<script>
import TypePokemon from '../Pokemon/TypePokemon'
import ImagePokemon from '../Pokemon/ImagePokemon'
import AbilitiesPokemon from '../Pokemon/AbilitiesPokemon'
import MovesPokemon from '../Pokemon/MovesPokemon'

export default {
  name: 'Modals',
  props:{
    details: {}
  },
  components:{
    TypePokemon,
    ImagePokemon,
    AbilitiesPokemon,
    MovesPokemon
  },
  methods: {
    close() {
      this.$emit('close');
    },
  },
  filters:{
      formatNumber(value){
          return "#"+(""+value).padStart(3, '0')
      }          
  },
};
</script>

<style scoped>
@import '../../assets/css/keyframes/rotationBackground.css';
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: var(--black30off);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1;
  }
  .modal {
    background: var(--lightgray2);
    box-shadow: 0px 0px 10px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    width: 400px;
    height: 600px;
    position: relative;
    border-radius: 25px;
    overflow: hidden;
  }
  .modal-body {
    position: relative;
  }
  .btn-close {
    border: none;
    font-size: 20px;
    cursor: pointer;
    font-weight: bold;
    color: var(--close-color);
    background: var(--lightgray2);
    right: 0px;
    position: absolute;
    z-index: 999;
    border-bottom-left-radius: 100px;
    padding: 5px 15px 15px 20px;
  }  
  .header-details{
    height: 40vh;
    margin-top: -45px;
  }
  .header-details p{
    padding-top: 50px;
    color: #FFF;
    display: flex;
    justify-content: center;
  }
  .header-details h1 {
    text-align: center;
    text-transform: capitalize;
    color: var(--white);
    text-shadow: 1px 1px var(--black);
    font-size: xxx-large;
    margin:0px;
    margin-top: -25px;
    z-index: 10!important;
    position: relative;
  }
  .typesDetails {
    display: inline-flex;
    text-align: center;
    width: 95%;
    justify-content: center;
    margin-top: 75px;
    font-size: 16px;
    z-index: 9;
  }
  .typesDetails div {
    border: 1px solid;
    margin: 10px;
    border-radius: 20px;
    padding: 0px 18px;
    background-color: var(--lightgray);
  }
  .body-details{
    min-height: 100px;    
    border-top-left-radius: 50px;
    border-top-right-radius: 50px;
    margin-top: -45px;
    background-color: var(--lightgray2);
    position: relative;
  }
  .body-details::before {
    content: "";
    position: absolute;
    width: 250px;
    height: 250px;
    left: 85px;
    top: -125px;
    z-index: 9!important;
    display: flex;
    justify-content: center;
    background-image: url('../../assets/images/pokeball.png');
    background-size: 100%;
    background-repeat: no-repeat;
    background-position: center center;
    will-change: animation, transform;
    -webkit-animation: rotationBackground 10s linear infinite;
    animation: rotationBackground 10s linear infinite;
  }
  .imageDetails{
    display: flex!important;
    justify-content: center!important;
    width: 200px!important;
    height: 200px!important;
    margin: auto!important;
    left: 20px!important;
    z-index: 10!important;
    background: none;
  }  
</style>