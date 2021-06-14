<template>
  <div class="dropdown-wrapper">
    <!-- Same thing, better not use Index as key in real practice -->
    <DropdownItem
      v-for="(option, index) in options"
      :key="index"
      :option="option"
    />
  </div>
</template>

<script>
import DropdownItem from "@/components/Dropdown/DropdownItem";
export default {
  name: "Dropdown",
  components: {
    DropdownItem,
  },
  props: {
    options: {
      type: [Array, Object],
      required: true,
    },
  },
};
</script>

<style scoped></style>
