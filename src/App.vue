<template>
  <div id="app">
    <Header/>
    <Menu 
      v-bind:filter="filter"
      v-bind:types="types"
      v-bind:sort="sort"
S    />
    <Results 
      v-bind:sorted="sorted"
    />
    <Footer/>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Menu from './components/Menu.vue';
import Results from './components/Results.vue';
import pokedex from './services/pokedex.js';


export default {
  name: 'app',
  data() {
    return {
      pokedex,
      filter: {
        type: 'all',
        attack: '',
        defense: ''
      },
      sort: {
        props: 'id'
      }
    };
  },
  components: {
    Header, Menu, Results, Footer
  },
  computed: {
    filtered() {
      const { type, attack, defense } = this.filter;
      const typeFiltered = this.pokedex.slice().filter(pokemon => type === 'all' || pokemon.type_1 === type || pokemon.type_2 === type);
      const attackFiltered = typeFiltered.filter(pokemon => pokemon.attack > attack);
      const defenseFiltered = attackFiltered.filter(pokemon => pokemon.defense > defense);
      return defenseFiltered;
    },
    sorted() {
      const { props } = this.sort;
      return this.filtered.slice().sort((a, b) => {
        const propA = a[props];
        const propB = b[props];
        if(propA > propB) return 1;
        if(propA < propB) return -1;
        return 0;
      });
    },
    types() {
      const type1 = this.pokedex.map(pokemon => pokemon.type_1);
      const type2 = this.pokedex.map(pokemon => pokemon.type_2);
      const allTypes = new Set(type1.concat(type2));
      return [...allTypes.values()];
    },
  }
};
</script>

<style scoped>






</style>
