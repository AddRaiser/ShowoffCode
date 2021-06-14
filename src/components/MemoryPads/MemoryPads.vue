<template>
  <div class="game-container">
    <div
      class="pads-wrapper"
      :class="[gridClass, { 'pads-wrapper--success': gameEnd }]"
    >
      <!-- Should not do that in real case. Index as key is bad, but I wont change pad colors in game so I can do it.      -->
      <Pad
        ref="pad"
        v-for="(color, index) in currentPadsColors"
        :key="index"
        :color="color"
        @click.native="handlePadClick(index)"
      />
    </div>
    <div class="menu-btn" @click="showMenu = true">Start new</div>
    <MemoryMenu v-show="showMenu" @new-game="startNewGame($event)" />
  </div>
</template>

<script>
import Pad from "@/components/MemoryPads/Pad";
import MemoryMenu from "@/components/MemoryPads/MemoryMenu";

export default {
  name: "MemoryPads",
  components: {
    Pad,
    MemoryMenu,
  },
  data: () => {
    return {
      showMenu: false,
      colorsList: [
        "black",
        "purple",
        "grey",
        "lime",
        "yellow",
        "cyan",
        "red",
        "green",
        "blue",
      ],
      currentPadsColors: [],
      openedPadsIndex: [],
      guessedPadsIndex: [],
    };
  },
  mounted() {
    this.startNewGame("Easy");
  },
  methods: {
    handlePadClick: function (index) {
      if (this.guessedPadsIndex.includes(index)) return;

      if (index === this.openedPadsIndex[0]) {
        this.$refs.pad[index].turnAround();
        this.openedPadsIndex = [];
        return;
      }

      if (this.openedPadsIndex.length >= 2) {
        this.openedPadsIndex.forEach((padIndex) =>
          this.$refs.pad[padIndex].turnAround()
        );
        this.openedPadsIndex = [];
      }

      this.$refs.pad[index].turnAround();
      this.openedPadsIndex.push(index);

      if (
        this.openedPadsIndex.length === 2 &&
        this.currentPadsColors[this.openedPadsIndex[0]] ===
          this.currentPadsColors[this.openedPadsIndex[1]]
      ) {
        this.guessedPadsIndex.push(...this.openedPadsIndex);
        this.openedPadsIndex = [];
      }
    },
    startNewGame: function (difficulty) {
      this.showMenu = false;
      this.guessedPadsIndex = [];
      this.openedPadsIndex = [];

      let colors = [];
      switch (difficulty) {
        case "Easy": {
          colors = this.colorsList.slice(-2);
          break;
        }
        case "Medium": {
          colors = this.colorsList.slice(-8);
          break;
        }
        case "Hard": {
          colors = this.colorsList.concat(this.colorsList);
          break;
        }
        default: {
          colors = this.colorsList.slice(-2);
          break;
        }
      }
      this.currentPadsColors = [...colors, ...colors];
      this.bestArrayShuffle(this.currentPadsColors);
      if (this.$refs && this.$refs.pad) {
        this.$refs.pad.forEach((padRef) => padRef.forceClose());
      }
    },
    bestArrayShuffle: function (array) {
      for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    },
  },
  computed: {
    gridClass: function () {
      return "pads-wrapper--" + Math.sqrt(this.currentPadsColors.length);
    },
    gameEnd: function () {
      return (
        this.currentPadsColors.length &&
        this.currentPadsColors.length === this.guessedPadsIndex.length
      );
    },
  },
};
</script>

<style scoped lang="scss">
.game-container {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 300px;
}

.pads-wrapper {
  display: grid;
  gap: 5px;
  padding: 5px;
  border: 5px solid transparent;
  border-radius: 10px;

  &--success {
    border-color: green;
  }

  &--2 {
    grid-template: repeat(2, auto) / repeat(2, auto);
  }

  &--4 {
    grid-template: repeat(4, auto) / repeat(4, auto);
  }

  &--6 {
    grid-template: repeat(6, auto) / repeat(6, auto);
  }
}
</style>
