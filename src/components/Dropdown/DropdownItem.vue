<template>
  <div @click="handleClick">{{ text }}</div>
</template>

<script>
export default {
  name: "DropdownItem",
  props: {
    option: {
      type: [String, Object],
      required: true,
    },
  },
  methods: {
    handleClick: function () {
      this.$emit("click", this.value || this.text);
    },
  },
  computed: {
    text: function () {
      return typeof this.option === "string" ? this.option : this.option.text;
    },
    value: function () {
      return typeof this.option === "string" ? this.option : this.option.value;
    },
  },
};
</script>

<style scoped></style>
