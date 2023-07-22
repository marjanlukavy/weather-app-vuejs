<template>
  <div class="weather-app-container">
    <AddBlockButton :weatherBlocks="weatherBlocks" :addBlock="addBlock" />
    <div v-for="(block, index) in weatherBlocks" :key="index">
      <WeatherBlock
        :block="block"
        :index="index"
        :fetchCitySuggestions="fetchCitySuggestions"
        :selectCity="selectCity"
        :addCity="addCity"
      />
    </div>
  </div>
</template>

<script>
import AddBlockButton from "./AddBlockButton.vue";
import TemperatureChart from "./TemperatureChart.vue";
import WeatherBlock from "./WeatherBlock.vue";

import { Chart, registerables } from "chart.js";

Chart.register(...registerables);
export default {
  components: {
    AddBlockButton,
    TemperatureChart,
    WeatherBlock,
    TemperatureChart,
  },
  data() {
    return {
      weatherBlocks: [
        {
          cityQuery: "",
          citySuggestions: [],
          selectedCity: null,
          weatherData: null,
          forecastData: null,
          chart: null,
        },
      ],
    };
  },

  methods: {
    addBlock() {
      this.weatherBlocks.push({
        cityQuery: "",
        citySuggestions: [],
        selectedCity: null,
        weatherData: null,
        forecastData: null,
        chart: null,
      });
    },

    async fetchCitySuggestions(index) {
      const response = await fetch(
        `https://cors-anywhere.herokuapp.com/https://maps.googleapis.com/maps/api/place/autocomplete/json?input=${this.weatherBlocks[index].cityQuery}&types=geocode&key=AIzaSyBE0ecqqVoZbLrajQTSy9bPUOvSqMjKlkE`,
        {
          headers: {
            "x-requested-with": "XMLHttpRequest",
          },
        }
      );

      const data = await response.json();
      this.weatherBlocks[index].citySuggestions = data.predictions;
    },

    selectCity(prediction, index) {
      const cityName = prediction.description.split(",")[0];
      this.weatherBlocks[index].selectedCity = cityName;
      this.weatherBlocks[index].cityQuery = cityName;
      this.weatherBlocks[index].citySuggestions = [];
    },

    async addCity(index) {
      if (this.weatherBlocks[index].selectedCity) {
        // const response = await fetch(
        //   `https://api.openweathermap.org/data/2.5/weather?q=${this.weatherBlocks[index].selectedCity}&appid=47664db9ed993c39673177a097d6d936`
        // );

        const forecastResponse = await fetch(
          `http://api.openweathermap.org/data/2.5/forecast?q=${this.weatherBlocks[index].selectedCity}&appid=47664db9ed993c39673177a097d6d936`
        );

        const forecastData = await forecastResponse.json();

        this.weatherBlocks[index].forecastData = forecastData;

        this.weatherBlocks[index].weatherData = this.processForecastData(
          forecastData.list
        );

        console.log(this.weatherBlocks[index].weatherData[0]);
      }
    },

    processForecastData(forecastList) {
      let groupedByDate = {};
      forecastList.forEach((item) => {
        let date = new Date(item.dt * 1000).toISOString().split("T")[0];
        if (!groupedByDate[date]) {
          groupedByDate[date] = [];
        }
        groupedByDate[date].push(item);
      });

      let dailyTemperatures = [];
      for (let date in groupedByDate) {
        let dailyData = groupedByDate[date];
        let minTemp = Math.min(...dailyData.map((item) => item.main.temp_min));
        let maxTemp = Math.max(...dailyData.map((item) => item.main.temp_max));
        dailyTemperatures.push({ date, minTemp, maxTemp });
      }

      return dailyTemperatures;
    },
  },
};
</script>

<style scoped>
.weather-app-container {
  background-color: white;
  padding: 10px 20px;
  display: flex;
  flex-direction: column;
  gap: 25px;
}
</style>
