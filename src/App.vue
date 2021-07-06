<template>
  <div id="app">
    <select name="" id="" v-model="sekcja" style="margin-bottom:20px">
      <option value="ART1K">ART1K</option>
      <option value="ART2K">ART2K</option>
      <option value="macro1K">macro1K</option>
      <option value="macro2K">macro2K</option>
      <option value="generator">generator</option>

    </select>


    <ART1K v-if="sekcja=='ART1K'"></ART1K>
    <ART2K v-if="sekcja=='ART2K'"></ART2K>
    <macro1K v-if="sekcja=='macro1K'"></macro1K>
    <macro2K v-if="sekcja=='macro2K'"></macro2K>
    <generator v-if="sekcja=='generator'"></generator>



  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue';
import ART1K from './components/ART1K.vue';
import ART2K from './components/ART2K.vue';
import macro1K from './components/macro1K.vue';
import macro2K from './components/macro2K.vue';
import generator from './components/GENERATORDECCO.vue';





export default {
  data(){
    return{
      sekcja:'ART1K',
      tryb:'ART1K'
    }
  },
  name: 'App',
  components: {
    ART1K,ART2K,macro1K,macro2K,generator
  },
  watch:{
    sekcja(val){
      localStorage.sekcja  = val
    }
  },
  mounted(){

if (localStorage.sekcja){
  this.sekcja = localStorage.sekcja
}
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
