<script lang="ts" setup>
import { ref, onMounted } from "vue";
import type { Ref } from "vue";

type WeatherData = {
  location: { name: string; region: string };
  current: { temp_c: number; condition: { text: string; icon: string } };
};

type Coords = { latitude: number; longitude: number };

const props = defineProps<{ coords: Coords }>();
const data: Ref<WeatherData | undefined> = ref();

const fetchWeather = async (coords: Coords): Promise<void> => {
  const { latitude, longitude } = coords;
  const response = await fetch(
      `https://api.weatherapi.com/v1/current.json?key=${
          import.meta.env.VITE_APP_WEATHER_API_KEY
      }&q=${latitude},${longitude}`
  );
  data.value = await response.json();
};

onMounted(() => fetchWeather(props.coords));
</script>

<template>
  <div v-if="data">
    <h1>Weather in {{ data.location.name }}, {{ data.location.region }}</h1>
    <p>{{ data.current.condition.text }} - {{ data.current.temp_c }}°C</p>
    <img :src="data.current.condition.icon" />
  </div>
  <div v-else>Loading...</div>
</template>
