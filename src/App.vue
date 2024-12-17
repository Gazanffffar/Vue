<script setup lang="ts">
import { ref } from "vue";
import GetLocation from "./components/GetLocation.vue";
import WeatherReport from "./components/WeatherReport.vue";

// Интерфейс для типизации геолокации
interface Geolocation {
  latitude: number;
  longitude: number;
}

// Переменная для хранения координат
const coords = ref<Geolocation | null>(null);

// Функция для установки координат с типом
const setCoords = (geolocation: Geolocation) => {
  coords.value = geolocation;
};
</script>

<template>
  <GetLocation @update:coords="setCoords" />
  <WeatherReport v-if="coords" :coords="coords" />
</template>
