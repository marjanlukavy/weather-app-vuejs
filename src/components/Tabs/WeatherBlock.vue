<template>
  <div class="weather-block">
    <CitySelection
      :block="block"
      :index="index"
      :fetchCitySuggestions="fetchCitySuggestions"
      :selectCity="selectCity"
      :addCity="addCity"
    />
    <DayWeekToggle />
    <WeatherCardContainer
      :weatherData="block.weatherData"
      @date-selected="changeSelectedDate"
    />
    <TemperatureChart
      :weatherBlock="block"
      :day="selectedDate"
      :daytime="'night'"
    />
  </div>
</template>

<script>
import CitySelection from "./CitySelection.vue";
import DayWeekToggle from "./DayWeekToggle.vue";
import WeatherCardContainer from "./WeatherCardContainer.vue";
import TemperatureChart from "./TemperatureChart.vue";

export default {
  components: {
    CitySelection,
    DayWeekToggle,
    WeatherCardContainer,
    TemperatureChart,
  },
  props: {
    block: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    fetchCitySuggestions: {
      type: Function,
      required: true,
    },
    selectCity: {
      type: Function,
      required: true,
    },
    addCity: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      selectedDate: null,
    };
  },
  methods: {
    changeSelectedDate(date) {
      let day = new Date(date).getDate();
      this.selectedDate = day;
      console.log(day);
    },
  },
};
</script>

<style scoped>
.weather-block {
  background-color: #526d82;
  padding: 15px;
}
</style>
