<template>
  <div class="tabs-container">
    <div class="tabs-nav">
      <TabButton
        :label="'Головна'"
        :active="tab === 'tab1'"
        @click="tab = 'tab1'"
      />
      <TabButton
        :label="'Обране'"
        :active="tab === 'tab2'"
        @click="tab = 'tab2'"
      />
    </div>

    <MainTab v-if="tab === 'tab1'" />
    <FavoritesTab v-if="tab === 'tab2'" />
  </div>
</template>

<script>
import { ref } from "vue";
import TabButton from "./TabButton.vue";
import MainTab from "./MainTab.vue";
import FavoritesTab from "./FavoritesTab.vue";

export default {
  components: {
    TabButton,
    FavoritesTab,
    MainTab,
  },
  setup() {
    const tab = ref("tab1");

    return {
      tab,
    };
  },
};
</script>

<style scoped>
.tabs-container {
  display: flex;
  flex-direction: column;
}

.tabs-nav {
  display: flex;
  gap: 10px;
}
</style>
