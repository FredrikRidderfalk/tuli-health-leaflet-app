<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <Map msg="Welcome habibi, do you happen to see a map?"/>

  <!-- Map -->
  <div class="map" id="mapid">
    <l-map 
      v-model="zoom"
      v-model:zoom="zoom"
      :center="[47.41322, -1.219482]"
      @move="log('move')">
      <l-geo-json :geojson="geojson" :options="geojsonOptions" />
    </l-map>
  </div>
</template>

<script>
// DON'T load Leaflet components here!
// Its CSS is needed though, if not imported elsewhere in your application.
import { LMap, LGeoJson } from "@vue-leaflet/vue-leaflet"
import "leaflet/dist/leaflet.css"

// John tutorial
import leaflet from "leaflet"

import Map from './components/Map.vue'
import { onMounted } from '@vue/runtime-core'
// Not sure which import, above or below, is correct
// import { onMounted } from 'vue'


export default {
  name: 'App',
  components: {
    Map,
    LMap,
    LGeoJson,
  },
  setup() {
    let mymap;

    onMounted(() => {
      mymap = leaflet.map('mapid').setView([51.505, -0.09], 13);

      leaflet.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiZGVlcGl0eSIsImEiOiJja3d0ZXF5OXMxZm5wMnBxbzdoNXJ6bHRqIn0.a2pkiB6fYydkqkzqFstnHA', {
      attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
      maxZoom: 18,
      id: 'mapbox/streets-v11',
      tileSize: 512,
      zoomOffset: -1,
      accessToken: 'pk.eyJ1IjoiZGVlcGl0eSIsImEiOiJja3d0ZXF5OXMxZm5wMnBxbzdoNXJ6bHRqIn0.a2pkiB6fYydkqkzqFstnHA'
}).addTo(mymap);
    })
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.map { 
  height: 50vh;
  width: 50vw;
  margin: 0 auto;
  }
</style>
