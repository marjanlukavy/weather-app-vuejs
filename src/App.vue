<template>
  <div class="main-container">
    <WeatherTabs msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import WeatherTabs from "./components/Tabs/WeatherTabs.vue";

export default {
  name: "App",
  components: {
    WeatherTabs,
  },
};
</script>

<style>
.main-container {
  max-width: 1200px;
  margin: 50px auto;
}
</style>
