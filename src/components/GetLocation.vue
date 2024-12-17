<script setup lang="ts">
import { ref, onMounted } from "vue";

type Geolocation = { latitude: number; longitude: number };

const coords = ref<Geolocation | null>(null);
const geolocationBlockedByUser = ref<boolean>(false);

const getGeolocation = async (): Promise<void> => {
  await navigator.geolocation.getCurrentPosition(
      (position) => {
        coords.value = {
          latitude: position.coords.latitude,
          longitude: position.coords.longitude,
        };
      },
      (error) => {
        geolocationBlockedByUser.value = true;
        console.error(error.message);
      }
  );
};

onMounted(getGeolocation);
</script>

<template>
  <div v-if="coords">
    <p>Latitude: {{ coords.latitude }}</p>
    <p>Longitude: {{ coords.longitude }}</p>
  </div>
  <div v-if="geolocationBlockedByUser">
    User denied access to location.
  </div>
</template>
