<template>
  <div class="game-container">
    <div class="pads" :class="gameEndClass">
      <SnakePads v-for="tile in 64" :key="tile" :value="tileType(tile - 1)" />
    </div>
    <div class="menu-btn" @click="startGame">New game</div>
  </div>
</template>

<script>
import SnakePads from "@/components/Snake/SnakePads";

export default {
  name: "Snake",
  components: {
    SnakePads,
  },
  data: () => {
    return {
      snake: [27, 26, 25],
      direction: "right",
      food: -1,
      movementTimer: null,
      movementIterationTime: 500,
      gameEndClass: "",
    };
  },
  mounted() {
    this.startGame();
  },
  methods: {
    startGame: function () {
      this.endGame();

      this.lastDirection = "left";
      this.direction = "right";
      this.snake = [27, 26, 25];
      this.food = -1;
      this.movementIterationTime = 500;
      this.gameEndClass = "";

      document.addEventListener("keydown", this.handleKeydown);
      this.movementTimer = setTimeout(
        this.movementIteration,
        this.movementIterationTime
      );
      this.spawnNewFood();
    },
    endGame: function (success = false) {
      clearTimeout(this.movementTimer);
      document.removeEventListener("keydown", this.handleKeydown);
      this.gameEndClass = success ? "pads-win" : "pads-lose";
    },
    movementIteration: function () {
      let newHeadTile = this.getNewHeadTile();
      this.lastDirection = this.direction;

      if (this.checkCollision(newHeadTile)) {
        this.endGame();
        return;
      }

      this.snake.unshift(newHeadTile);
      if (newHeadTile === this.food) {
        this.spawnNewFood();
      } else {
        this.snake.pop();
      }

      this.movementTimer = setTimeout(
        this.movementIteration,
        this.movementIterationTime
      );
    },

    spawnNewFood: function () {
      let emptyTiles = Array(64)
        .fill("")
        .map((elem, index) => index);
      emptyTiles = emptyTiles.filter((elem) => !this.snake.includes(elem));

      if (emptyTiles.length) {
        this.food = emptyTiles[Math.floor(Math.random() * emptyTiles.length)];

        if (emptyTiles.length % 8 === 0) {
          this.movementIterationTime *= 0.9;
        }
      } else {
        this.endGame(true);
      }
    },
    checkCollision: function (newHeadTile) {
      return (
        newHeadTile < 0 ||
        newHeadTile > 63 ||
        (Math.floor(newHeadTile / 8) !== Math.floor(this.snake[0] / 8) &&
          ["left", "right"].includes(this.direction)) ||
        this.snake.includes(newHeadTile)
      );
    },
    getNewHeadTile: function () {
      if (this.direction === "bottom") return this.snake[0] + 8;
      if (this.direction === "left") return this.snake[0] - 1;
      if (this.direction === "top") return this.snake[0] - 8;
      if (this.direction === "right") return this.snake[0] + 1;
    },
    handleKeydown: function (event) {
      if ([83, 40].includes(event.keyCode) && this.lastDirection !== "top") {
        event.preventDefault();
        this.direction = "bottom";
      } else if (
        [65, 37].includes(event.keyCode) &&
        this.lastDirection !== "right"
      ) {
        event.preventDefault();
        this.direction = "left";
      } else if (
        [87, 38].includes(event.keyCode) &&
        this.lastDirection !== "bottom"
      ) {
        event.preventDefault();
        this.direction = "top";
      } else if (
        [68, 39].includes(event.keyCode) &&
        this.lastDirection !== "left"
      ) {
        event.preventDefault();
        this.direction = "right";
      }
    },
    tileType: function (index) {
      if (this.snake.includes(index)) {
        if (this.snake[0] === index) return "snake-head";
        return "snake";
      }

      if (this.food === index) return "food";
      return "";
    },
  },
};
</script>

<style scoped lang="scss">
.game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 auto;
}

.pads {
  display: grid;
  grid-template: repeat(8, auto) / repeat(8, auto);
  gap: 1px;
  border: 5px solid transparent;

  &-win {
    border-color: green;
  }

  &-lose {
    border-color: red;
  }
}
</style>
