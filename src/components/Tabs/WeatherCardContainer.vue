<template>
  <div class="weather-card-container">
    <WeatherCard
      v-for="(data, index) in weatherData"
      :key="index"
      :weatherData="data"
      @date-selected="$emit('date-selected', $event)"
    />
  </div>
</template>

<script>
import WeatherCard from "./WeatherCard.vue";

export default {
  components: {
    WeatherCard,
  },
  props: ["weatherData"],
};
</script>

<style scoped>
.weather-card-container {
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}
</style>
