<template>
  <div>
    <img alt="Pikachu"  class="pikachu" src="../../assets/images/pikachu.gif" />         
  </div>
</template>

<script>
export default {
    name:'Pikachu',
    data(){
      return {
        urlImage : ''
      }
    }
}
</script>

<style>
  .pikachu{
    position: absolute;
    width: 100px;
    height: 80px;
    top: 450px;
    left: 5vw;
  }
</style>