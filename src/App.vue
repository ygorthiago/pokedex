<template>
  <div id="app">
    <br>
    <img src="./assets/pokedex.png" alt="Placeholder image" width="300" height="300">

    <div class="column is-half is-offset-one-quarter">

      <input type="text" class="input is-rounded" placeholder="Search pokemon by name" v-model="search">
      <button class="button is-fullwidth is-success" id="search-btn" @click="find">Catch!</button>
      <div v-for="(poke,index) in filteredPokemons" :key="poke.url">      
            <Pokemon :name="poke.name" :url="poke.url" :num="++index" />
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/pokemon';

export default {
  name: 'App',
  data(){
    return{
      pokemons: [],
      filteredPokemons: [],
      search:''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {      
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  components:{
    Pokemon
  },
  methods: {
    find: function(){
      this.filteredPokemons = this.pokemons;
      if(this.search == '' || this.search == ' '){
          this.filteredPokemons = this.pokemons;
      } else {
          this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.search);
      }

    }
  },
  computed: {
    // searchResult: function (){
    //   if(this.search == '' || this.search == ' '){
    //     return this.pokemons;
    //   } else {
    //     return this.pokemons.filter(pokemon => pokemon.name == this.search);
    //   }
    // }
  }
}
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    
    background-color:#2c3e50;
  }
  #search-btn{
    margin-top: 1%;
  }
</style>
