<template>
  <div class="weather-card" @click="$emit('date-selected', weatherData.date)">
    <h2>{{ formatDate(weatherData.date) }}</h2>
    <p>Min Temp: {{ toCelsius(weatherData.minTemp) }} °C</p>
    <p>Max Temp: {{ toCelsius(weatherData.maxTemp) }} °C</p>
  </div>
</template>

<script>
export default {
  props: ["weatherData"],
  methods: {
    toCelsius(kelvin) {
      return (kelvin - 273.15).toFixed(2);
    },
    formatDate(dateString) {
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let date = new Date(dateString);
      let day = days[date.getDay()];
      return `${date.getDate()} ${day}`;
    },
  },
};
</script>

<style scoped>
.weather-card {
  border-radius: 15px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
  background: linear-gradient(
    135deg,
    #1e5799 0%,
    #2989d8 50%,
    #207cca 51%,
    #7db9e8 100%
  );
  color: #fff;
  padding: 20px;
  margin: 10px;
  font-family: "Roboto", sans-serif;
  text-align: center;
  transition: 0.3s ease-in-out;
  cursor: pointer;
}

.weather-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 20px rgba(0, 0, 0, 0.2);
}
.weather-card h2 {
  font-size: 1.5em;
  margin-bottom: 0.5em;
}
.weather-card p {
  font-size: 1em;
}
</style>
