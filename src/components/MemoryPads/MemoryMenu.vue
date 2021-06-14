<template>
  <div class="menu-wrapper">
    <div class="menu">
      <p>Difficulty</p>
      <div
        class="menu-option"
        :class="{ 'menu-option--chosen': chosenDifficulty === difficulty }"
        v-for="difficulty in difficulties"
        :key="difficulty"
        @click="chosenDifficulty = difficulty"
      >
        {{ difficulty }}
      </div>

      <div class="menu-confirm" @click="handleConfirmClick">Start new game</div>
    </div>
    <div class="menu-overlay"></div>
  </div>
</template>

<script>
export default {
  name: "MemoryMenu",
  data: () => {
    return {
      chosenDifficulty: "Easy",
      difficulties: ["Easy", "Medium", "Hard"],
    };
  },
  methods: {
    handleConfirmClick: function () {
      this.$emit("new-game", this.chosenDifficulty);
    },
  },
};
</script>

<style scoped lang="scss">
.menu {
  max-height: 90%;
  padding: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #ccc;
  margin: 10px;
  z-index: 1;
  font-size: 20px;
  font-weight: bolder;
  text-transform: uppercase;
  color: #333;
  border-radius: 10px;

  p {
    margin-bottom: 10px;
  }

  &-wrapper {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
  }

  &-overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background: black;
    opacity: 0.9;
    z-index: 0;
  }

  &-option {
    background: #282828;
    color: white;
    padding: 6px 10px;
    margin: 5px 0;
    border-radius: 10px;
    cursor: pointer;

    &--chosen {
      background: grey;
    }
  }

  &-confirm {
    margin-top: 10px;
    padding: 6px 10px;
    border-bottom: 3px solid #ccc;
    cursor: pointer;

    &:hover {
      border-bottom-color: #333;
    }
  }
}
</style>
