<template>
  <div class="temperature-chart-container">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
import { Chart, registerables } from "chart.js";
Chart.register(...registerables);

export default {
  props: {
    weatherBlock: {
      type: Object,
      required: true,
    },
    daytime: {
      type: String,
      required: true,
    },
    day: {
      type: Number,
      required: true,
    },
  },
  mount() {
    console.log(this.weatherBlock);
  },
  methods: {
    formatWeatherData() {
      if (
        !this.weatherBlock.forecastData ||
        !this.weatherBlock.forecastData.list
      ) {
        return {
          labels: [],
          datasets: [
            {
              label: "Temperature",
              data: [],
              fill: false,
              borderColor: "rgb(75, 192, 192)",
              tension: 0.1,
            },
          ],
        };
      }

      const filteredData = this.weatherBlock.forecastData.list
        .filter((item) => {
          let dt_txt = item.dt_txt;
          let dateParts = dt_txt.split(" ");
          let date = dateParts[0];
          let time = dateParts[1];

          let dateElements = date.split("-");
          let dayOfMonth = parseInt(dateElements[2], 10);
          let hour = parseInt(time.split(":")[0], 10);

          return (
            (this.daytime === "morning" && hour >= 6 && hour < 17) ||
            (this.daytime === "night" && hour >= 18 && hour < 24)
          );
        })
        .slice(0, 3);

      const labels = filteredData.map((item) => {
        // Extracting the time portion from the dt_txt field
        const time = item.dt_txt.split(" ")[1].slice(0, 5);
        return time;
      });

      const data = filteredData
        .map((item) => item.main.temp - 273.15)
        .slice(0, 3);

      return {
        labels,
        datasets: [
          {
            label: "Temperature",
            data,
            fill: false,
            borderColor: "rgb(75, 192, 192)",
            tension: 0.1,
          },
        ],
      };
    },
    createChart() {
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext("2d");
      const chartData = this.formatWeatherData();

      if (this.weatherBlock.chart) {
        this.weatherBlock.chart.destroy();
      }

      this.weatherBlock.chart = new Chart(ctx, {
        type: "line",
        data: chartData,
        options: {
          // Chart.js options...
        },
      });
    },
    updateChart() {
      if (this.weatherBlock.chart) {
        this.weatherBlock.chart.data = this.formatWeatherData();
        this.weatherBlock.chart.update();
      }
    },
  },
  watch: {
    day: {
      handler() {
        this.updateChart();
      },
      immediate: true,
    },
    "weatherBlock.forecastData": {
      deep: true,
      handler() {
        this.createChart();
      },
    },
  },
};
</script>

<style scoped>
canvas {
  width: 100%;
  height: auto;
}
</style>
