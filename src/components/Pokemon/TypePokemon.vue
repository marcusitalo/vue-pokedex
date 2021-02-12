<template>
  <div class="infoTypes">
      <div v-for="(type, index) in this.types" :key="index" >
          {{index === 0 ? setBackground(type) : null}}
          <p v-bind:style="{ background : colors[type.type.name] }" >{{type.type.name}}</p>
      </div>
  </div>
</template>

<script>
export default {
    data(){
        return {
            colors : {
                normal: '#A8A77A',
                fire: '#EE8130',
                water: '#6390F0',
                electric: '#F7D02C',
                grass: '#7AC74C',
                ice: '#96D9D6',
                fighting: '#C22E28',
                poison: '#A33EA1',
                ground: '#E2BF65',
                flying: '#A98FF3',
                psychic: '#F95587',
                bug: '#A6B91A',
                rock: '#B6A136',
                ghost: '#735797',
                dragon: '#6F35FC',
                dark: '#705746',
                steel: '#B7B7CE',
                fairy: '#D685AD'
            },
        }
    },
    props:{        
        types : Array,
    },
    methods:{
        setBackground: function(data){
            this.$emit("applyBackgroundColor",{ color : this.colors[data.type.name] })
        }
    }
}
</script>

<style scoped>
    .infoTypes {
        position: absolute;
        top: 20px;
        left: 15px;
        text-transform: capitalize 
    }
    .infoTypes p {
        padding: 1px 5px;
        border-radius: 10px;
        margin: 3px 0px;
        color:var(--white);
        border: 1px solid var(--white);
    }
</style>