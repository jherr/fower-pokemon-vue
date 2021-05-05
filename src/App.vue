<template>
  <div p-10 maxW-1200 m="auto">
    <input
      p-5
      text4XL
      border-1
      roundedXL
      borderGray500
      w="100%"
      v-model="filter"
    />
    <div
      grid
      gap-10
      mt-10
      class="layout"
    >
      <PokemonCard
        v-for="p in filtered"
        :key="p.id"
        :pokemon="p"
        @select="onSelect"
      />
    </div>
  </div>
</template>

<script>
import PokemonCard from './components/PokemonCard';

export default {
  name: 'App',
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemon: [],
      filter: "",
      selected: [],
      filtered: []
    }
  },
  watch: {
    filter() {
      this.updateFiltered();
    }
  },
  mounted() {
    this.fetchPokemon()
  },
  methods: {
    async fetchPokemon() {
      fetch('/pokemon.json')
        .then(resp => resp.json())
        .then(data => {
          this.pokemon = data;
          this.updateFiltered();
        })
    },
    updateFiltered() {
      const lcFilter = this.filter.toLowerCase();
      this.filtered = this.pokemon
        .filter(({ name: { english }}) => english.toLowerCase().includes(lcFilter))
        .map((p) => ({
          ...p,
          selected: this.selected.includes(p.name.english),
        }))
        .slice(0,10);
    },
    onSelect(name) {
      if(this.selected.includes(name)) {
        this.selected = this.selected.filter((n) => n !== name);
      } else {
        this.selected = [...this.selected, name];
      }
    }
  }
}
</script>

<style>
body {
  font-family: Arial, Helvetica, sans-serif;
}

@media only screen and (max-width: 800px) {
  .layout {
    grid-template-columns: repeat(1, minmax(0px, 1fr));
  }
}
@media only screen and (min-width: 800px) {
  .layout {
    grid-template-columns: repeat(2, minmax(0px, 1fr));
  }
}
</style>
