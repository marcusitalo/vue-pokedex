<template>
  <ul class="listPagination" :style="{'--rotation': rotationPokeball }">     
    <li v-for="index in endPage" :key="index" @click="updatePages(index)" 
      :class="{'currentPage': index == current}" 
        v-show='
          (index == 1 || index == endPage) ||           
          (
            (current <= (viewIndex + 1) && index <= viewIndex * 2 ) ||             
            (current > endPage - viewIndex && index > (endPage - ( viewIndex * 2 ))) ||           
            (
              (current > (viewIndex + 1) && current <= endPage - viewIndex) && 
              ((index > (current - viewIndex)) && 
              index < (current + viewIndex))
            )
          )' >
      {{ index }}
    </li>    
  </ul>  
</template>

<script>
export default {
  name: 'Pagination',
  data(){
    return {
      viewIndex : 5,
      rotationPokeball:'rotate(25deg)'
    }
  },
  props:{
    endPage:Number,
    current:Number,
  },
  methods: {
    updatePages: function(page){
      this.rotationPokeball = (page > this.current) ? 'rotate(25deg)' : 'rotate(-25deg)'
      this.$emit("updatePages",{ page })
    }
  },  
};
</script>

<style scoped>
  ul.listPagination {
    display: flex;
    list-style: none;
    margin: auto;
    justify-content: center;
    margin-bottom: 30px;
    width: 50vw;
    flex-wrap: wrap;    
    padding: 0px;
  }
  ul.listPagination li {
    margin: 1px;
    cursor: pointer;
    text-align: center;
    position: relative;
    color: var(--gray);
    font-family: monospace;    
    transition: 2s;
  }
  ul.listPagination li::after {
    content: "";
    display: flex;
    justify-content: center;
    width: 20px;
    height: 15px;
    background-repeat: no-repeat;
    background-image: url('../../assets/images/not-selected.png');
    background-size: 70% 100%;
    background-position: center;
    margin: auto;    
    transition: 2s;
  } 
  li.currentPage {
    transition: 2s;
    color: var(--white)!important;
  } 
  li.currentPage::after {
     background-image: url('../../assets/images/selected.png')!important;
     transform: var(--rotation);
     transition: 2s;
  }
</style>