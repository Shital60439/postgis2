<template>
  <main class="w-screen h-screen">
    <v-map class="w-full h-full" :options="data.options" @loaded="onMapLoaded" />
  </main>
</template>




<script setup lang="ts">
import mapboxgl from "mapbox-gl";
import VMap from "v-mapbox";
const data = reactive({
  options: {
    accessToken:
      "pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng",
    style: "mapbox://styles/mapbox/streets-v11?optimize=true",
    center: [73.8567, 18.5204],
    zoom: 11,
    maxZoom: 22,
    crossSourceCollisions: false,
    failIfMajorPerformanceCaveat: false,
    attributionControl: false,
    preserveDrawingBuffer: true,
    hash: false,
    minPitch: 0,
    maxPitch: 60,
   
  } as mapboxgl.MapboxOptions,
  mappoints:[],
});
async function onMapLoaded(map: mapboxgl.Map) {
  console.log("map loaded");
  data.mappoints = await $fetch('http://localhost:3004/Addpoint');
   console.log("array data",data.mappoints);
  // var latlongpoints = [];
   data.mappoints.forEach((e) => {
    console.log("foreach",e)
    let lon = e.lon;
    let lati = e.lat;
    // latlongpoints.push(lon, lati);
    new mapboxgl.Marker({
                draggable: true,
                color: "#" + (Math.random().toString(16) + "000000").substring(2, 8),
            })
      .setLngLat([lon, lati])
      .addTo(map);
  });

  map.addSource('india', {
        type: 'geojson',
              data: {
        "type": "FeatureCollection",
        "features": [
          {
            "type": "Feature",
            "properties": {},
            "geometry": {
              "type": "Polygon",
              "coordinates": [
                [
                  [
                    -29.53125,
                    46.558860303117164
                  ],
                  [
                    -21.09375,
                    14.944784875088372
                  ],
                  [
                    25.3125,
                    38.54816542304656
                  ],
                  [
                    -29.53125,
                    46.558860303117164
                  ]
                ]
              ]
            }
          }
        ]
      }

    });
    map.addLayer({
        'id': 'india',
        'type': 'fill',
        'source': 'india', // reference the data source
        'layout': {},
        'paint': {
            'fill-color': '#0080FF', // blue color fill
            'fill-opacity': 0.5
        }
    });
}
  
</script>

























<style>
html,
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2C3E50;
}
.w-screen {
  width: 100vw;
}
.h-screen {
  height: 100vh;
}
.h-full {
  height: 100%;
}
.w-full {
  width: 100%;
}
</style>