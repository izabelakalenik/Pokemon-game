<template>
  <Map 
    :playerX="internalPlayerX" 
    :playerY="internalPlayerY" 
    @onEnteredGrass="handleEnteredGrass"
    @onExitedGrass="handleExitedGrass" 
    @onStartedMoving="handleStartedMoving"
    @onFinishedMoving="handleFinishedMoving" 
  />
</template>

<script>
import Map from '../base_components/Map.vue';
import { blocks, isBlockWalkable } from "@/globals";

export default {
  components: {
    Map,
  },
  emits: ['isInWildEncounter', 'internalPlayerX', 'internalPlayerY', 'isMoving'],
  props: {
    playerX: Number,
    playerY: Number,
    isInMenu: Boolean,
    isInPokedex: Boolean,
    isInWildEncounter: Boolean,
    isMoving: Boolean,
    movementUp: Boolean,
    movementDown: Boolean,
    movementLeft: Boolean,
    movementRight: Boolean,
  },
  data() {
    return {
      internalPlayerX: this.playerX,
      internalPlayerY: this.playerY,
    };
  },
  watch: {
    movementUp(newValue) {
      console.log('movementUp');
      this.updatePosition('up', newValue);
    },
    movementDown(newValue) {
      this.updatePosition('down', newValue)
    },
    movementLeft(newValue) {
      this.updatePosition('left', newValue)
    },
    movementRight(newValue) {
      this.updatePosition('right', newValue)
    },
    playerX(newValue) {
      this.internalPlayerX = newValue;
    },
    playerY(newValue) {
      this.internalPlayerY = newValue;
    },
  },
  methods: {
    handleEnteredGrass() {
      if (!this.isInWildEncounter && !this.isInMenu && !this.isInPokedex) {
        fetch("http://localhost:8081/enter_grass")
          .then((response) => {
            if (response.status == 400) {
              // this.isInWildEncounter = true;
              this.$emit("isInWildEncounter", true); // Player encountered a wild Pokemon
            }
          })
          .catch((error) => {
            console.error("Error:", error);
          });
      }

    },
    handleExitedGrass() {
      if (!this.isInWildEncounter && !this.isInMenu && !this.isInPokedex) {
        fetch("http://localhost:8081/leave_grass")
          .then((response) => {
            if (response.status === 200) {
              console.log("Player exited grass successfully!");
            }
          })
          .catch((error) => {
            console.error("Error:", error);
          });
      }
    },

    handleStartedMoving() {
      this.$emit("isMoving", true);
    },

    handleFinishedMoving() {
      this.$emit("isMoving", false);
    },

    updatePosition(direction, newValue) {
      if (newValue && (!this.isMoving || !this.isInWildEncounter || !this.isInMenu || !this.isInPokedex)) {

        let newX = this.internalPlayerX;
        let newY = this.internalPlayerY;

        switch (direction) {
          case "up":
            newY -= 1;
            break;
          case "down":
            newY += 1;
            break;
          case "left":
            newX -= 1;
            break;
          case "right":
            newX += 1;
            break;
        }

        if (newY >= 0 && newY < blocks.length && newX >= 0 && newX < blocks[0].length && isBlockWalkable[blocks[newY][newX]]) {
          this.internalPlayerX = newX;
          this.internalPlayerY = newY;
          this.$emit("internalPlayerX", newX);
          this.$emit("internalPlayerY", newY);
        }
      }
    },
  },
};
</script>