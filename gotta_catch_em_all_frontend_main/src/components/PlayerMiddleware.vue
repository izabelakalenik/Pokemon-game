<template>
  <Player 
    :facing="internalFacingDirection" 
    :movementType="movementType"
  />
</template>

<script>
import Player from '../base_components/Player.vue'; 


export default {
  components: {
    Player,
  },
  emits: ['internalFacingDirection'],
  props: {
    facingDirection: String,
    isMoving: Boolean,
    isInMenu: Boolean,
    isInWildEncounter: Boolean,
    isInPokedex: Boolean,
    movementUp: Boolean,
    movementDown: Boolean,
    movementLeft: Boolean,
    movementRight: Boolean,
  },
  data() {
    return {
      internalFacingDirection: this.facingDirection,
      movementType: 'standing',
    };
  },
  watch: {
    movementUp(newValue) {
      this.updateFacingDirection('up');
      this.updateMovementType(newValue)
    },
    movementDown(newValue) {
      this.updateFacingDirection('down');
      this.updateMovementType(newValue)
    },
    movementLeft(newValue) {
      this.updateFacingDirection('left');
      this.updateMovementType(newValue)
    },
    movementRight(newValue) {
      this.updateFacingDirection('right');
      this.updateMovementType(newValue)
    },
    isMoving(newValue){
      console.log('isMoving', newValue);
      this.updateMovementType(newValue);
    },
    isInMenu(newValue){
      this.updateMovementType(false);
    }
  },
  methods: {
    updateFacingDirection(direction) {
      if (!this.isMoving || !this.isInWildEncounter || !this.isInMenu || !this.isInPokedex) {

        let newDirection = this.internalFacingDirection;

        switch (direction) {
          case "up":
            newDirection = 'north';
            break;
          case "down":
            newDirection = 'south';
            break;
          case "left":
            newDirection = 'west';
            break;
          case "right":
            newDirection = 'east';
            break;
        }

        this.internalFacingDirection = newDirection;
        this.$emit("internalFacingDirection", newDirection);
      }
    },
    updateMovementType(newValue) {
      console.log('1', this.isMoving);
      console.log('2', this.isInWildEncounter);
      console.log('3', this.isInPokedex);
      if (this.isMoving || newValue) {
        this.movementType = "walking";
      } else if (this.isInMenu || !this.isInWildEncounter || this.isInPokedex) {
        this.movementType = "standing";
      } else {
        this.movementType = "standing";
      }
    },
  },
};
</script>