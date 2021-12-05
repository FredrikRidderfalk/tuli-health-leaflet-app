<template>
  <header class="header">
    <img alt="Vue logo" src="./assets/logo.png" class="logo">
  </header>

  <Map msg="Locate Nearby Pharmacies For Medical Appointments And Testing"/>
  <label for="post-code">Enter your post code here: </label>
  <input 
    v-model="queryPostCode"
    id="post-code" 
    placeholder="E.g. SW7 3DX" 
  />
  <button @Click="getPostCodeInfo" class="locate-btn">Locate</button>

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
import { onMounted, ref } from '@vue/runtime-core'
import axios from "axios"
// Not sure which import, above or below, is correct
// import { onMounted } from 'vue'

// const existingPostCodes = [
//   "EC2A 3AG",
//   "N1 1RA",
//   "W12 9BL",
//   "W1U 6AA",
//   "SW7 3HZ",
//   "SW7 3DX",
//   "W11 4UA",
//   "SW13 9LB",
//   "HP9 2JH",
//   "HP9 1QD",
//   "N1 2UQ",
//   "W2 2HU",
//   "W4 5DG",
//   "NW5 2HR",
//   "W11 2SE",
//   "W8 6QD",
//   "W11 3HL",
//   "OL2 8NP",
//   "W11 1LA",
//   "NW3 2PT"
// ]


export default {
  name: 'App',
  components: {
    Map,
    LMap,
    LGeoJson,
  },
  setup() {
    let mymap;
    const queryPostCode = ref("");
    const postCodeInfo = ref(null);

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

// not sure about the get endpoint...
    const getPostCodeInfo = async () => {
      try {
        const data = await axios.get(`https://api.postcodes.io/postcodes/${queryPostCode.value}`)
      const result = data.data;
      // console log result so that we can see that our post codes fetches the API data
      // console.log(result)
      postCodeInfo.value={
        latitude: result.latitude,
        longitude: result.longitude,
      }
      // now we can use postCodeInfo when we need the latitude and longitude to show a pin on the map. Display the info on the page with the prop v-bind: postCodeInfo="postCodeInfo"
      }
      catch(err) {
        alert(err.message)
      }
    }
    return { queryPostCode, postCodeInfo, getPostCodeInfo };
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  letter-spacing: 1.1px;
  background: linear-gradient(160deg, rgba(243,251,255,1) 50%, rgba(248,246,255,1) 100%);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #5c3b8a;
  margin-top: 60px;
}

.map { 
  min-height: 60vh;
  width: 100vw;
  margin: 0 auto;
  }

.logo {
  max-width: 162px;
  margin-left: 15px;
  height: 100%;
}

.header {
  display: flex;
  justify-content: space-between;
}

.locate-btn {
  border: none;
  background-color: #5c3b8a;
  padding: 5px 15px;
  color: white;
  font-weight: 600;
}

input {
  padding: 3px;
}
</style>
