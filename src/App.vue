<template>
  <div>
    <div>
      <figure>
        <img src="./assets/images/pokedex.png" class="banner"/>
      </figure>
    </div>
    <div class="searchSection">
      <input type="text" name="" id="" placeholder="Pesquisar pokemon. Ex:Charizard" v-model="search" class="searchBar" @keyup="debounce"/>      
    </div>
    <div id="app" v-if="allPokemons">
      <Pikachu />  
      <div class='listsPokemons'>
        <div v-for="(pokemon,index) in filteredPokemons" :key="pokemon.url">
          <CardPokemon :name="pokemon.name" :url="pokemon.url" :number="index + 1" @showDetails="showModal($event)" />
        </div>   
      </div>           
    </div> 
    <div>
      <Modal v-if="isModalVisible" @close="closeModal" :details="details"/>    
    </div>
  </div>
</template>

<script>
import Pikachu from './components/Animation/Pikachu'
import CardPokemon from './components/Pokemon/CardPokemon'
import Modal from './components/Modal/Details'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Pikachu,
    CardPokemon,
    Modal
  },
  data(){
    return {
      allPokemons : [],
      filteredPokemons : [],
      search: "",
      timeDelay: null,
      isModalVisible: false,
      details: {},
    }
  },  
  created(){
    if(localStorage.pokemons){
        this.allPokemons = JSON.parse(localStorage.pokemons)
        this.filteredPokemons = JSON.parse(localStorage.pokemons) 
        return;      
    }
    else{
       this.getInApi();
    }
  },  
  methods:{
    getInApi: async function(){
        await axios.get(`${process.env.VUE_APP_URL_POKEAPI}pokemon?limit=${process.env.VUE_APP_LIMIT}&offset=${process.env.VUE_APP_OFSSET}`)
          .then(res => {
            this.allPokemons = res.data.results
            this.filteredPokemons = res.data.results
            localStorage.pokemons = JSON.stringify(res.data.results)
          })
    },
    showModal(data) {
      this.details = data
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    debounce: function(){      
      clearTimeout(this.timeDelay)
      this.timeDelay = this.filtedPokemon(this.search)              
    },
    filtedPokemon:function(search){
      this.timeDelay = setTimeout(() => {
        if(search.trim() == ''){
          this.filteredPokemons = this.allPokemons
        }else{
          this.filteredPokemons = this.allPokemons.filter(pokemon => pokemon.name.toUpperCase().indexOf(search.toUpperCase()) != -1)
        }
      }, 1000)          
    }
  }  
}
</script>

<style>
@import './assets/css/keyframes/movementBackground.css';

  body{
    background-image: url('./assets/images/background.jpg');
    background-size: 100vw 650px;  
    background-repeat: repeat-x;
    background-color:var(--ligth-green);
    will-change: animation;
    animation: movementBackground 20s linear infinite; 
    cursor: url('./assets/cursor/cursor.png'),auto;
  }
  #app {
    font-family: Monospace, Arial, sans-serif;
    text-align: center;
  }
  .banner{
    width: 200px;
    display: flex;
    justify-content: center;
    margin: auto;
  }
  .searchSection{
    display: flex;
    margin-bottom: 30px;
  }
  .searchBar{
    margin: auto;
    width: 50%;
    border: 0px;
    padding: 10px;
    background: var(--lightblue-searchbar);
    border-bottom: 3px solid var(--lightblue-searchbar-border);
    text-align: center;
    box-shadow: 0px 0px 3px 1px var(--lightblue-searchbar-shadow);
    border-radius: 20px;
  }
  .searchBar::placeholder { 
    color: var(--lightblue-searchbar-shadow);
  }
  .listsPokemons {
      display: flex;
      width: 80vw;
      flex-wrap: wrap;
      margin: auto;
      align-items: center;
      justify-content: center;
  }  
</style>
