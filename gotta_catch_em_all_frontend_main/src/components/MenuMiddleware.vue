<template>
  <Menu 
    v-if="isInMenu" 
    :state="state" 
    :selectorPos="selectorPos" 
  />
</template>

<script>
import Menu from '../base_components/Menu.vue';

export default {
  components: {
    Menu,
  },
  emit: ['inInMenu', 'isInPokedex'],
  props: {
    isInMenu: Boolean,
    isInWildEncounter: Boolean,
    isInPokedex: Boolean,
    playerX: Number,
    playerY: Number,
    facingDirection: String,
    pokemonRegister: Object,
    isButtonA: Boolean,
    isButtonB: Boolean,
    movementUp: Boolean,
    movementDown: Boolean,
  },
  data() {
    return {
      selectorPos: 0,
      state: 'START',
      savedData: {
        x: this.playerX,
        y: this.playerY,
        direction: this.facingDirection,
        pokedex: this.pokemonRegister,
      },
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
      this.updateSelectorPos('up', newValue)
    },
    movementDown(newValue) {
      this.updateSelectorPos('down', newValue)
    },
  },
  methods: {
    updateState(button, isPressed) {
      if(isPressed) {
        if ((button == 'B' && this.state == 'START') || (button == 'A' && this.selectorPos == 2)) {
          this.$emit('isInMenu', false)
        } else if (button == 'A' && this.selectorPos == 1) {
          this.state = 'SAVING'
          this.saveGame();
        } else if (button == 'A' && this.selectorPos == 0){
          this.$emit('isInPokedex', true)
        } else if (button == 'B' && this.isInPokedex == true){
          this.$emit('isInPokedex', false)
          this.state == 'START'
          this.$emit('isInMenu', true)
          //TO DO - DOESN'T WORK
        } else if (this.state == 'SAVED') {
          this.$emit('isInMenu', false)
        }
      }
    },
    updateSelectorPos(direction, isPressed) {
      if (isPressed && this.isInMenu && this.state != 'SAVING' && !this.isInPokedex) {
        if (this.selectorPos == 0 && direction == 'up') {
          this.selectorPos = 2;
        } else if (this.selectorPos == 2 && direction == 'down') {
          this.selectorPos = 0;
        } else if (direction == 'up') {
          this.selectorPos -= 1;
        } else {
          this.selectorPos += 1;
        }
      }
    },
    saveGame() {
      fetch("http://localhost:8081/save", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(this.savedData),
      }).catch((error) => {
        console.error("Error:", error);
      });

      this.state = 'SAVED'
    },
  },
};
</script>