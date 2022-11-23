<script setup lang="ts">
import { Loader } from "@googlemaps/js-api-loader";
import { ref, onMounted, onUnmounted } from "vue";
import { currentLocation } from "@/stores/currentLocationStore";

const mapDiv = ref<HTMLDivElement | null>(null);

// eslint-disable-next-line no-undef
let clickListener: google.maps.MapsEventListener;
// eslint-disable-next-line no-undef
let map: google.maps.Map;
// eslint-disable-next-line no-undef
let marker: google.maps.Marker;

const loader = new Loader({
  apiKey: import.meta.env.VITE_GOOGLE_MAPS_API_KEY,
  version: "weekly",
});

const mapOptions = {
  center: currentLocation.value,
  zoom: 8,
};

onMounted(async () => {
  await loader.load();
  if (mapDiv.value) {
    // eslint-disable-next-line no-undef
    map = new google.maps.Map(mapDiv.value, mapOptions);
  } else {
    //Log error.
  }

  // eslint-disable-next-line no-undef
  marker = new google.maps.Marker({
    position: currentLocation.value,
    map: map,
    draggable: true,
  });

  // eslint-disable-next-line no-undef
  clickListener = marker.addListener(
    "drag",
    // eslint-disable-next-line no-undef
    (e: google.maps.MapMouseEvent) => {
      if (e.latLng) {
        currentLocation.value = {
          lat: e.latLng.lat(),
          lng: e.latLng.lng(),
        };
      }
    }
  );
});

onUnmounted(() => {
  // eslint-disable-next-line no-undef
  google.maps.event.removeListener(clickListener);
});
</script>

<template>
  <div ref="mapDiv"></div>
</template>

<style scoped>
div {
}
</style>
