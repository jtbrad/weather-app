<script setup lang="ts">
import MapHolder from "@/components/MapHolder.vue";
import WeatherData from "@/components/WeatherData.vue";
</script>

<template>
  <main>
    <h1>Current Weather</h1>
    <MapHolder class="mapHolder" />
    <WeatherData class="weatherData" />
  </main>
</template>

<style scoped>
main {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 100px;
  width: 100%;
}

h1 {
  grid-column: 1 / 5;
  grid-row: 1 / 2;
}

.mapHolder {
  grid-column: 1 / 4;
  grid-row: 2 / 7;
}

.weatherData {
  grid-column: 4 / 5;
  grid-row: 2 / 7;
}
</style>
