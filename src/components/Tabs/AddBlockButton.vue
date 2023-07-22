<template>
  <button @click="addBlock" v-if="weatherBlocks.length < 5" class="">+</button>
</template>

<script>
export default {
  props: ["weatherBlocks", "addBlock"],
};
</script>
