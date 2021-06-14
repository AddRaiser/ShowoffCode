<template>
  <div ref="pad" class="pad"></div>
</template>

<script>
export default {
  name: "Pad",
  props: {
    color: {
      type: String,
      required: true,
    },
  },
  data: () => {
    return {
      isOpened: false,
    };
  },
  methods: {
    turnAround: function () {
      this.$refs.pad.classList.add("pad--turning");
      setTimeout(() => {
        if (!this.$refs.pad) return;

        this.$refs.pad.style.backgroundColor = this.isOpened
          ? "#ccc"
          : this.color;
        this.isOpened = !this.isOpened;
        this.$refs.pad.classList.remove("pad--turning");
      }, 100);
    },
    forceClose: function () {
      if (this.isOpened) this.turnAround();
    },
  },
};
</script>

<style scoped lang="scss">
.pad {
  width: 50px;
  height: 50px;
  background: #ccc;
  border: 5px solid white;
  border-radius: 10px;
  transition: transform 0.1s linear;

  &--turning {
    transform: scaleX(0);
  }
}
</style>
