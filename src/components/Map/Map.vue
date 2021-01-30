<template>
  <div class="map">
    <l-map
      :center="setMapCenter"
      :zoom="setMapZoom"
    >
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      ></l-tile-layer>
      <l-control-layers />
      <l-circle
        v-for="country in countries" :key="country"
        :lat-lng="[country.countryInfo.lat, country.countryInfo.long]"
        :radius="setRadius(country.cases, circleCases.multiplayer)"
        :color="circleRecovered.color"
        :fill="true"
        :fillColor="circleRecovered.color"
        :fillOpacity="0.2"
      />
    </l-map>
  </div>
</template>
<script>
// Import leaflet components to show map with circle markers
import {
  LMap,
  LTileLayer,
  LCircle,
  LControlLayers
} from "@vue-leaflet/vue-leaflet";

// Import default css for the
import "leaflet/dist/leaflet.css";

export default {
  components: {
    LMap,
    LTileLayer,
    LCircle,
    LControlLayers
  },
  props:{
    setMapCenter: Array,
    setMapZoom: Number,
    countries: String
  },
  data() {
    return {
      circleCases: {
        color: '#CC1034',
        multiplayer: 800,
      },
      circleRecovered:{
        color: '#7dd71d',
        multiplayer: 1200
      },
      circleDeaths:{
        color: '#fb4443',
        multiplayer: 2000
      }
    };
  },
  methods:{
    setRadius(country, multiplayer){
      return Math.sqrt(JSON.stringify(country)) * multiplayer / 2
    }
  }
};
</script>
<style lang="scss">
    .map{
        width: 100%;
        height: 500px;
        border-radius: 20px;
        background-color: white;
        margin-top: 16px;
        padding: 1rem;
        box-shadow: 0 0 8px -4px rgba(0,0,0,0.5);
    }
</style>