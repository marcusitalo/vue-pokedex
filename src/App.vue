<template>
  <div>
    <div>
      <figure>
        <img src="./assets/images/pokedex.png" class="banner"/>
      </figure>
    </div>
    <div class="searchSection">
      <div class='viewSearch'>
        <input type="text" name="" id="searchBar" placeholder="Pesquisar pokemon. Ex:Charizard" v-model="search" class="searchBar" @keyup="debounce"/>            
        <span for="searchBar" v-if="search" class="btn-clean" @click="cleanSearchBar">X</span>
      </div>      
    </div>
    <div id="app" v-if="allPokemons">
      <Pikachu />        
      <div class='listsPokemons'>
        <div v-for="(pokemon,index) in filteredPokemons" :key="pokemon.url">
          <CardPokemon :slug="pokemon.name" :url="pokemon.url" :number="index + 1" @showDetails="showModal($event)" />
        </div>   
      </div>           
    </div> 
    <div>
      <Modal v-if="isModalVisible" @close="closeModal" :details="details"/>    
    </div>
    <div>
      <Pagination :endPage="allPages" :current="currentPage" @updatePages="changePage($event)" />
    </div>
  </div>
</template>

<script>
import Pikachu from './components/Animation/Pikachu'
import CardPokemon from './components/Pokemon/CardPokemon'
import Modal from './components/Modal/DetailsPokemon'
import Pagination from './components/Pagination/Painel'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Pikachu,
    CardPokemon,
    Modal,
    Pagination
  },
  data(){
    return {
      allPokemons : [],
      filteredPokemons : [],
      search: "",
      timeDelay: null,
      isModalVisible: false,
      details: {},
      allPages:1,
      perPage: 15,
      currentPage: 1,
    }
  },  
  created(){
    if(localStorage.pokemons){
        this.allPokemons = JSON.parse(localStorage.pokemons)
        this.filteredPokemons = JSON.parse(localStorage.pokemons).slice(0, this.perPage)        
    }
    else{
       this.getInApi();
    }
    this.setPaginationTotal()
  },  
  methods:{
    getInApi: async function(){
        await axios.get(`${process.env.VUE_APP_URL_POKEAPI}pokemon?limit=${process.env.VUE_APP_LIMIT}&offset=${process.env.VUE_APP_OFSSET}`)
          .then(res => {
            this.allPokemons = res.data.results
            this.filteredPokemons = res.data.results.slice(0, this.perPage)
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
    cleanSearchBar(){
      this.search = ''
      this.debounce()
    },    
    filtedPokemon:function(search){
      this.timeDelay = setTimeout(() => {
        const paginationPokemons = (search.trim() == '') ? this.allPokemons : this.allPokemons.filter(pokemon => pokemon.name.toUpperCase().indexOf(search.toUpperCase()) != -1)          
        this.setCurrentPage(1)
        this.setAllPage(Math.ceil(paginationPokemons.length/this.perPage))
        this.filteredPokemons = paginationPokemons.slice(0,this.perPage)
      }, 1000)          
    },
    debounce: function(){      
      clearTimeout(this.timeDelay)
      this.timeDelay = this.filtedPokemon(this.search)              
    },
    setAllPage: function(value){      
      this.allPages = value             
    },
    setCurrentPage: function(value){      
      this.currentPage = value             
    },
    setPaginationTotal(){
      this.allPages = Math.ceil(this.allPokemons.length/this.perPage);
    },    
    changePage(data){
      this.paginationPokemon(data.page);
    },    
    paginationPokemon:function(page){
      this.setCurrentPage(page)
      const offset = (page-1) * this.perPage
      const paginationPokemons = (this.search.trim() == '') ? this.allPokemons : this.allPokemons.filter(pokemon => pokemon.name.toUpperCase().indexOf(this.search.toUpperCase()) != -1)
      this.filteredPokemons = paginationPokemons.slice(offset, (offset + this.perPage))
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
    margin: 30px 0px;    
    justify-content: center;
  }  
  .searchBar{
    margin: auto;
    width: 50vw;
    border: 0px;
    padding: 10px;
    background: var(--lightblue-searchbar);
    border-bottom: 3px solid var(--lightblue-searchbar-border);
    text-align: center;
    box-shadow: 0px 0px 3px 1px var(--lightblue-searchbar-shadow);
    border-radius: 20px;
    font-family: monospace;
    font-weight: bolder;
    color: var(--lightblue-searchbar-shadow);
    font-size: larger;
  }
  .searchBar::placeholder { 
    color: var(--lightblue-searchbar-shadow);
  }
  .viewSearch {
      display: inline-block;
  }
  .btn-clean {
    position: absolute;
    margin: 7px -30px;
    font-family: cursive;
    font-size: 15px;
    font-weight: bold;
    color: var(--lightblue-clean-text);
    text-shadow: 1px 1px 1px var(--lightblue-clean-shadow);
    cursor: pointer;
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
