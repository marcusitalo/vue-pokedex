<template>
  <div class="moves" :style="{'--scroll-color': scrollColor }" >
      <h2>Moves</h2>
      <div class="listMoves">
        <div v-for="(move, index) in this.moves" :key="index" >
            <p>{{move.move.name}}</p>
        </div>
      </div>
  </div>
</template>

<script>
export default {
    props:{        
        moves : Array,
        scrollColor: String
    }
}
</script>

<style>
    .movesDetails {
        position: absolute;
        width: 99%;
        text-align: center;
        top: 215px;
    }
    .movesDetails h2{  
        margin:5px;
    }
    .listMoves {
        display: flex;
        justify-content: left;
        overflow-x: auto;
        margin: 5px;
        padding: 10px 0px;
        text-transform: capitalize;
    }
    .listMoves p {
        border: 1px solid;
        padding: 2px 10px;
        margin: 2px;
        border-radius: 20px;
        width: 100px;
    }
    .listMoves::-webkit-scrollbar {
        width: 5px;
        height: 8px;
        background-color: var(--lightgray-scroolbar-bg);
        border: 1px solid var(--lightgray-scrollbar-border);
        border-radius: 20px;
    }
    .listMoves::-webkit-scrollbar-thumb {
        background: var(--scroll-color);
        border-radius: 10px;
        width: 100px;
    }
    
</style>