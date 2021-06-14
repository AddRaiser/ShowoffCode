<template>
  <div class="snake-pad" :class="value"></div>
</template>

<script>
export default {
  name: "SnakePads",
  props: {
    value: {
      value: String,
      required: true,
    },
  },
};
</script>

<style scoped lang="scss">
.snake-pad {
  width: 20px;
  height: 20px;
  background: #ccc;
}

.snake {
  background: green;

  &-head {
    background: red;
  }
}

.food {
  background: orange;
}
</style>
