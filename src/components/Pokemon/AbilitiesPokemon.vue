<template>
  <div class="abilities">
    <h2>Abilities</h2>
    <div class="listAbilities">
        <div v-for="(ability, index) in this.abilities" :key="index" >
            <p>{{ability.ability.name}}</p>
        </div>
    </div>
  </div>
</template>

<script>
export default {
    props:{        
        abilities : Array,
    }
}
</script>

<style scoped>
    .abilities.abilitiesDetails {
        position: absolute;
        width: 99%;
        text-align: center;
        top: 130px;
    }
    .abilitiesDetails h2{
        margin:5px;
    }
    .listAbilities {
        display: flex;
        justify-content: center;
        padding: 10px 0px;
        text-transform: capitalize;
    }
    .listAbilities p {
        border: 1px solid;
        padding: 2px 10px;
        margin: 2px;
        border-radius: 20px;
    }
</style>