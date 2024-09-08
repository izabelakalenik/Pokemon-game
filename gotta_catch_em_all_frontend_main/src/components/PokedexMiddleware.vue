<template>
  <Pokedex 
    v-if="isInPokedex" 
    :numberOfOwnedPokemons="numberOfOwnedPokemons"
    :numberOfSeenPokemons="numberOfSeenPokemons" 
    :pokemonsInfo="pokemonsInfo" 
  />
</template>

<script>
import Pokedex from '../base_components/Pokedex.vue';
import { pokemonNames } from "@/globals";

export default {
  emits: ['isInPokedex'],
  props: {
    isInPokedex: Boolean,
    pokemonRegister: Object,
    isButtonB: Boolean,
  },
  components: {
    Pokedex,
  },
  data() {
    return {
      numberOfOwnedPokemons: 0,
      numberOfSeenPokemons: 0,
      pokemonsInfo: [],
    }
  },
  watch: {
    isButtonB() {
      if (this.isInPokedex) {
        this.$emit('isInPokedex', false)
      }
    },
    isInPokedex(){
      if (this.isInPokedex) {
        this.handleNumberOfOwnedPokemons();
        this.handleNumberOfSeenPokemons();
        this.handlePokemonsInfo();
      }
    },
  },
  methods: {
    handleNumberOfOwnedPokemons() {
      this.numberOfOwnedPokemons = 0;
      for (const key in this.pokemonRegister) {
        if (this.pokemonRegister[key] == true) {
          this.numberOfOwnedPokemons++;
        }
      }
      console.log(this.numberOfOwnedPokemons)
    },
    handleNumberOfSeenPokemons() {
      this.numberOfSeenPokemons = Object.keys(this.pokemonRegister).length;
      console.log(this.numberOfSeenPokemons)
    },
    handlePokemonsInfo() {
      this.pokemonsInfo = [];
      for (const pokemonId in this.pokemonRegister) {
        const name = pokemonNames[pokemonId].name;
        const owned = pokemonRegister[pokemonId];
        this.pokemonsInfo.push({ id: parseInt(pokemonId), owned, name });
      }
      console.log(this.pokemonsInfo)
    }
  },
}
</script>