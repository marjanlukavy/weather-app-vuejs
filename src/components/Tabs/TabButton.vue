<template>
  <button class="tab" :class="{ active: active }" @click="$emit('click')">
    {{ label }}
  </button>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    active: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
.tab {
  border: none;
  padding: 20px;
  color: #f5f5f5;
  font-size: 20px;
  border-radius: 15px 15px 0 0;
  background-color: #4fc0d0;
  cursor: pointer;
}

.active {
  background-color: white;
  color: black;
}
</style>
