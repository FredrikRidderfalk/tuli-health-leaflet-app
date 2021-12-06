<template>
  <header class="header">
    <img alt="Tuli logo" src="./assets/logo.png" class="logo">
  </header>

  <label for="post-code">Enter your post code here: </label>
  <input 
    v-model="queryPostCode"
    id="post-code" 
    placeholder="E.g. SW7 3DX" 
  />
  <button @Click="getPostCodeInfo" class="locate-btn">Locate</button>

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
import { LMap, LGeoJson } from "@vue-leaflet/vue-leaflet"
import "leaflet/dist/leaflet.css"

import leaflet from "leaflet"

import { onMounted, ref } from '@vue/runtime-core'
import axios from "axios"

export default {
  name: 'App',
  components: {
    LMap,
    LGeoJson,
  },
  setup() {
    let mymap;
    const queryPostCode = ref("");
    const postCodeInfo = ref(null);
    // const existingPostCodeInfo = ref(null);
    // const existingPostCodes = {
    //   "postcodes": [
    //     "EC2A 3AG",
    //     "N1 1RA",
    //     "W12 9BL",
    //     "W1U 6AA",
    //     "SW7 3HZ",
    //     "SW7 3DX",
    //     "W11 4UA",
    //     "SW13 9LB",
    //     "HP9 2JH",
    //     "HP9 1QD",
    //     "N1 2UQ",
    //     "W2 2HU",
    //     "W4 5DG",
    //     "NW5 2HR",
    //     "W11 2SE",
    //     "W8 6QD",
    //     "W11 3HL",
    //     "OL2 8NP",
    //     "W11 1LA",
    //     "NW3 2PT"
    //   ]
    //   };

    const existingPostCodes = [
      {
        "latitude": 51.5249,
        "longitude": -0.0805,
      },
      {
        "latitude": 51.5402,
        "longitude": -0.1028,
      },
      {
        "latitude": 51.5028,
        "longitude": -0.2433,
      },
      {
        "latitude": 51.5199,
        "longitude": -0.1561,
      },
      {
        "latitude": 51.4936,
        "longitude": -0.1745,
      },
      {
        "latitude": 51.4935,
        "longitude": -0.1757,
      },
      {
        "latitude": 51.5068,
        "longitude": -0.2076,
      },
      {
        "latitude": 51.4733,
        "longitude": -0.2482,
      },
      {
        "latitude": 51.6019,
        "longitude": -0.6347,
      },
      {
        "latitude": 51.6104,
        "longitude": -0.6454,
      },
      {
        "latitude": 51.5428,
        "longitude": -0.1029,
      },
      {
        "latitude": 51.5173,
        "longitude": -0.1666,
      },
      {
        "latitude": 51.5018,
        "longitude": -0.2655,
      },
      {
        "latitude": 51.5561,
        "longitude": -0.1392,
      },
      {
        "latitude": 51.5142,
        "longitude": -0.2004,
      },
      {
        "latitude": 51.4966,
        "longitude": -0.1930,
      },
      {
        "latitude": 51.5097,
        "longitude": -0.1970,
      },
      {
        "latitude": 51.5165,
        "longitude": -0.2052,
      },
      {
        "latitude": 51.5545,
        "longitude": -0.1659,
      },
    ]

    onMounted(() => {
      // 51.522250, -0.081100 - lat and long for Tuli Health headquarters
      mymap = leaflet.map('mapid').setView([51.522250, -0.081100], 9);

      leaflet.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiZGVlcGl0eSIsImEiOiJja3d0ZXF5OXMxZm5wMnBxbzdoNXJ6bHRqIn0.a2pkiB6fYydkqkzqFstnHA', {
      attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
      maxZoom: 18,
      id: 'mapbox/streets-v11',
      tileSize: 512,
      zoomOffset: -1,
      accessToken: 'pk.eyJ1IjoiZGVlcGl0eSIsImEiOiJja3d0ZXF5OXMxZm5wMnBxbzdoNXJ6bHRqIn0.a2pkiB6fYydkqkzqFstnHA'
}).addTo(mymap);
    })

    const getPostCodeInfo = async () => {
      try {
        const data = await axios.get(`https://api.postcodes.io/postcodes/${queryPostCode.value}`)
        const result = data.data;
        console.log(result.result)
        postCodeInfo.value = {
          latitude: result.result.latitude,
          longitude: result.result.longitude,
        }


// the marker icon from the Leaflet library is currently broken, so we manually set it below
      let leafletIcon = leaflet.icon ({
        iconUrl: "https://unpkg.com/leaflet@1.7.1/dist/images/marker-icon-2x.png",
        shadowUrl: "https://unpkg.com/leaflet@1.7.1/dist/images/marker-shadow.png",

        iconSize:     [28, 46], // size of the icon
        shadowSize:   [50, 64], // size of the shadow
        iconAnchor:   [18, 75], // point of the icon which will correspond to marker's location
        shadowAnchor: [20, 93],  // the same for the shadow
        popupAnchor:  [-3, -76] // point from which the popup should open relative to the iconAnchor
      })
      
      // existingPostCodes will be an object, and this forEach loops through it
      existingPostCodes.forEach(code => {
        leaflet.marker([code.latitude, code.longitude], {icon: leafletIcon}).addTo(mymap);
      })

      leaflet.marker([postCodeInfo.value.latitude, postCodeInfo.value.longitude], {icon: leafletIcon}).addTo(mymap)
      .bindPopup(`<strong class="strong">${queryPostCode.value}</strong>`)
      .openPopup();

      mymap.setView([postCodeInfo.value.latitude, postCodeInfo.value.longitude], 13);
      }
      catch(err) {
        alert(err.message)
      }
    }
    return { queryPostCode, postCodeInfo, getPostCodeInfo, existingPostCodes };
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
  margin-top: 30px;
}

.map { 
  min-height: 85vh;
  width: 100vw;
  margin: 0 auto;
  margin-top: 10px;
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
  cursor: pointer;
  margin-left: 5px;
  border-radius: 2px;
}

.locate-btn:hover {
  background-color: #6f49a5;
}

.strong {
  text-transform: uppercase;
}

input {
  padding: 3px;
}
</style>
