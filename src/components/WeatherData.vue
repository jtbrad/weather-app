<script setup lang="ts">
import { currentLocation } from "@/stores/currentLocationStore";
import { onMounted, ref } from "vue";
import axios from "axios";

interface WeatherData {
  location: string;
  condition: string;
  temperature: number;
  humidity: number;
  sunrise: string;
  sunset: string;
  windSpeed: number;
  windDirection: number;
}

const weatherData = ref<WeatherData | null>(null);

function formatTime(time: number) {
  return new Date(time).toLocaleTimeString("en-US");
}

async function getWeatherData() {
  try {
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?lat=${
        currentLocation.value.lat
      }&lon=${currentLocation.value.lng}&appid=${
        import.meta.env.VITE_OPEN_WEATHER_API_KEY
      }&units=imperial`
    );
    console.log(response.data);
    weatherData.value = {
      location: response.data.name,
      condition: response.data.weather[0].description,
      temperature: response.data.main.temp,
      humidity: response.data.main.humidity,
      sunrise: formatTime(response.data.sys.sunrise),
      sunset: formatTime(response.data.sys.sunset),
      windSpeed: response.data.wind.speed,
      windDirection: response.data.wind.deg,
    };
  } catch (error) {
    console.error(error);
  }
}

onMounted(() => {
  getWeatherData();
});
</script>

<template>
  <div>
    <h2>Weather at Location</h2>
    <table v-if="weatherData">
      <tr>
        <td>Location:</td>
        <td>{{ weatherData.location }}</td>
      </tr>
      <tr>
        <td>Condition:</td>
        <td>{{ weatherData.condition }}</td>
      </tr>
      <tr>
        <td>Temperature:</td>
        <td>{{ weatherData.temperature.toFixed(2) }} F</td>
      </tr>
      <tr>
        <td>Humidity:</td>
        <td>{{ weatherData.humidity.toFixed(0) }}%</td>
      </tr>
      <tr>
        <td>Sunrise:</td>
        <td>{{ weatherData.sunrise }} UTC</td>
      </tr>
      <tr>
        <td>Sunset:</td>
        <td>{{ weatherData.sunset }} UTC</td>
      </tr>
      <tr>
        <td>Wind Speed:</td>
        <td>{{ weatherData.windSpeed }} mph</td>
      </tr>
      <tr>
        <td>Wind Direction:</td>
        <td>{{ weatherData.windDirection }} degrees</td>
      </tr>
    </table>
  </div>
</template>

<style scoped></style>
