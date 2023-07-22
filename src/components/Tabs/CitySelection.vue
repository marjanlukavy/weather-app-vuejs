<template>
  <div class="city-selection-container">
    <div class="input-container">
      <input
        type="text"
        v-model="block.cityQuery"
        @input="fetchCitySuggestions(index)"
      />
      <ul v-if="block.citySuggestions.length && block.cityQuery.length">
        <li
          v-for="prediction in block.citySuggestions"
          :key="prediction.place_id"
          @click="selectCity(prediction, index)"
        >
          {{ prediction.description }}
        </li>
      </ul>
    </div>
    <button @click="addCity(index)">Add</button>
  </div>
</template>

<script>
export default {
  props: ["block", "index", "fetchCitySuggestions", "selectCity", "addCity"],
};
</script>

<style scoped>
li {
  list-style: none;
}

ul {
  margin: 0;
  padding: 0;
}
.city-selection-container {
  display: flex;
  align-items: baseline;
  gap: 10px;
  margin-bottom: 20px;
}

.city-selection-container input {
  border: none;
  padding: 10px 15px;
  border-radius: 4px 4px 0 0;
  background-color: #f2f2f2;
}

.city-selection-container button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px 4px;
  background-color: #9db2bf;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.city-selection-container button:hover {
  background-color: #7a8f99;
}

.input-container {
  position: relative;
}

.input-container ul {
  position: absolute;
  width: 100%;
  background-color: #eeeded;
  border-radius: 0 0 4px 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  z-index: 1;
}

.input-container ul li {
  padding: 10px 15px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.input-container ul li:hover {
  background-color: #ddd;
}
</style>
