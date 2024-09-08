<template>
  <div>
    <ControlsFrame 
      @onMovementUp="handleMovementUp" 
      @onMovementDown="handleMovementDown"
      @onMovementLeft="handleMovementLeft" 
      @onMovementRight="handleMovementRight"
      @onButtonA="handleButtonA" 
      @onButtonB="handleButtonB" 
      @onButtonMenu="handleButtonMenu" 
      />
    <MapMiddleware 
      v-if="!isInWildEncounter && !isInPokedex" 
      :playerX="playerX" 
      :playerY="playerY"
      :isInPokedex="isInPokedex" 
      :isInMenu="isInMenu"
      @isInWildEncounter="handleWildEncounter" 
      @internalPlayerX="handlePlayerX"
      @internalPlayerY="handlePlayerY"
      :isMoving="isMoving"
      @isMoving="handleIsMoving" 
      :movementUp="movementUp"
      :movementDown="movementDown" 
      :movementLeft="movementLeft" 
      :movementRight="movementRight" 
      />
    <PlayerMiddleware 
      :facingDirection="facingDirection" 
      :isMoving="isMoving" 
      :isInMenu="isInMenu"
      :isInWildEncounter="isInWildEncounter" 
      :isInPokedex="isInPokedex"
      @updateFacingDirection="handleUpdateFacingDirection" 
      :movementUp="movementUp" 
      :movementDown="movementDown"
      :movementLeft="movementLeft" 
      :movementRight="movementRight" 
      />
    <WildEncounterMiddleware 
      v-if="isInWildEncounter" 
      :isInWildEncounter="isInWildEncounter"
      @isInWildEncounter="handleWildEncounter"
      :pokemonRegister="pokemonRegister"
      @internalPokemonRegister="handlePokemonRegister" 
      :isButtonA="isButtonA" 
      :isButtonB="isButtonB" 
      :movementUp="movementUp"
      :movementDown="movementDown"  
      />
    <MenuMiddleware 
      v-if="isInMenu" 
      :isInMenu="isInMenu" 
      @isInMenu="handleMenu" 
      :isInWildEncounter="isInWildEncounter"
      :isInPokedex="isInPokedex" 
      @isInPokedex="handlePokedex" 
      :playerX="playerX" 
      :playerY="playerY"
      :facingDirection="facingDirection" 
      :pokemonRegister="pokemonRegister" 
      :isButtonA="isButtonA"
      :isButtonB="isButtonB" 
      :movementUp="movementUp" 
      :movementDown="movementDown" 
      />
    <PokedexMiddleware 
      v-if="isInPokedex" 
      :isInPokedex="isInPokedex" 
      @isInPokedex="handlePokedex"
      :pokemonRegister="pokemonRegister" 
      :isButtonB="isButtonB" 
    />
  </div>
</template>

<script>
import ControlsFrame from './base_components/ControlsFrame.vue';
import MapMiddleware from './components/MapMiddleware.vue';
import PlayerMiddleware from './components/PlayerMiddleware.vue';
import WildEncounterMiddleware from './components/WildEncounterMiddleware.vue';
import MenuMiddleware from './components/MenuMiddleware.vue';
import PokedexMiddleware from './components/PokedexMiddleware.vue';

export default {
  name: 'App',
  components: {
    ControlsFrame,
    MapMiddleware,
    PlayerMiddleware,
    WildEncounterMiddleware,
    MenuMiddleware,
    PokedexMiddleware
  },
  data() {
    return {
      playerX: 0,
      playerY: 0,
      facingDirection: "south",
      pokemonRegister: {},
      isMoving: false,
      isInWildEncounter: false,
      isInPokedex: false,
      isInMenu: false,
      movementUp: false,
      movementDown: false,
      movementLeft: false,
      movementRight: false,
      isButtonA: false,
      isButtonB: false,
    };
  },
  mounted() {
    this.fetchInitialInfo();
  },
  methods: {
    fetchInitialInfo() {
      // Perform a GET request to the backend's /initial_info endpoint
      fetch("http://localhost:8081/initial_info")
        .then((response) => response.json())
        .then((data) => {
          // Extract the data from the response and update component data
          // Fetches initial player data (position, direction, Pokedex) from backend
          this.playerX = data.x;
          this.playerY = data.y;
          this.facingDirection = data.direction;
          this.pokemonRegister = data.pokedex;
        })
        .catch((error) => {
          console.error("Error fetching initial info:", error);
        });
    },
    handleMovementUp(isPressed) {
      console.log('movementUp');
      this.movementUp = isPressed;
    },
    handleMovementDown(isPressed) {
      console.log('movementDown');
      this.movementDown = isPressed;
    },
    handleMovementLeft(isPressed) {
      console.log('movementLeft');
      this.movementLeft = isPressed;
    },
    handleMovementRight(isPressed) {
      console.log('movementRight');
      this.movementRight = isPressed;
    },
    handleButtonA(isPressed) {
      this.isButtonA = isPressed;
    },
    handleButtonB(isPressed) {
        this.isButtonB = isPressed;
    },
    handleButtonMenu(isPressed) {
      if (isPressed) {
        this.isInMenu = !this.isInMenu;
      }
    },
    handleUpdateFacingDirection(newDirection) {
      this.facingDirection = newDirection;
    },
    handleMenu(isInMenu){
      this.isInMenu = isInMenu;
    },
    handleWildEncounter(isInWildEncounter) {
      this.isInWildEncounter = isInWildEncounter;
    },
    handlePokedex(isInPokedex) {
      this.isInPokedex = isInPokedex;
    },
    handlePlayerX(newValue) {
        this.playerX = newValue;
    },
    handlePlayerY(newValue) {
        this.playerY = newValue;
    },
    handleIsMoving(isMoving) {
      this.isMoving = isMoving;
    },
    handlePokemonRegister(newValue){
      this.pokemonRegister = newValue;
    },
  }
};
</script>
