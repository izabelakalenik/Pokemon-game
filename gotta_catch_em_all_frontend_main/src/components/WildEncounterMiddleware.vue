<template>
  <WildEncounter 
    v-if="isInWildEncounter" 
    :state="state" 
    :selectorPos="selectorPos" 
    :pokemonId="pokemonId"
    :isPokemonOwned="isPokemonOwned" 
  />
</template>

<script>
import WildEncounter from '../base_components/WildEncounter.vue';

export default {
  components: {
    WildEncounter,
  },
  emits: ['isInWildEncounter', 'internalPokemonRegister'],
  props: {
    isInWildEncounter: Boolean,
    pokemonRegister: Object,
    isButtonA: Boolean,
    isButtonB: Boolean,
    movementUp: Boolean,
    movementDown: Boolean,
  },
  data() {
    return {
      internalPokemonRegister: this.pokemonRegister,
      state: 'START',
      selectorPos: 0,
      pokemonId: 0,
      isPokemonOwned: false,
    };
  },
  watch: {
    isButtonA(newValue) {
      this.updateState('A', newValue)
    },
    isButtonB(newValue) {
      this.updateState('B', newValue)
    },
    movementUp(newValue) {
      this.updateSelectorPos(newValue)
    },
    movementDown(newValue) {
      this.updateSelectorPos(newValue)
    },
  },
  mounted() {
    this.determinePokemonId()
  },
  methods: {
    updateState(button, isPressed) {
      if (this.isInWildEncounter && isPressed) {
        if (this.state == 'START') {
          this.state = 'MAIN';
        } else if (this.state == 'MAIN' && button == 'A' && this.selectorPos == 0) {
          this.state = 'CATCHING'
          this.capturePokemon();
        } else if (this.state == 'MAIN' && button == 'A' && this.selectorPos == 1) {
          this.state = 'RUNNING_AWAY'
        } else {
          this.state = 'START';
          this.selectorPos = 0;
          this.$emit("isInWildEncounter", false);
        }
      }
    },
    updateSelectorPos(isPressed) {
      if (this.isInWildEncounter && isPressed) {
        if (this.selectorPos == 0) {
          this.selectorPos = 1;
        }
        else {
          this.selectorPos = 0;
        }
      }
    },
    capturePokemon() {
      fetch("http://localhost:8081/capture")
        .then((response) => {
          if (response.status === 200) {
            this.isPokemonOwned = true;
            this.internalPokemonRegister[this.pokemonId] = true;
            this.$emit('internalPokemonRegister', this.internalPokemonRegister);
            this.state = "CAUGHT";
          } else {
            this.state = "MAIN";
          }
        })
        .catch((error) => {
          console.error("Error:", error);
        });
    },
    determinePokemonId() {
      const randomValue = Math.floor(Math.random() * 10);
      const newPokemonId = randomValue < 5 ? 16 : 19;

      this.pokemonId = newPokemonId;

      if (!this.internalPokemonRegister[newPokemonId]) {
        this.internalPokemonRegister[newPokemonId] = false;
      }

      this.$emit('internalPokemonRegister', this.internalPokemonRegister)
    }
  },
};
</script>