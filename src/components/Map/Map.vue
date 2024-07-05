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
        :radius="setRadius(country, selectedType)"
        :color="handleInfoBoxChange(selectedType)"
        :fill="true"
        :fillColor="handleInfoBoxChange(selectedType)"
        :fillOpacity="0.2"
      >
        <l-popup class="popup">
          <img :src="country.countryInfo.flag" class="popup-flag"/>
          <h3 class="popup-countryName"> {{country.country }} </h3>
          <p class="popup-cases">Cases: {{ formatDigits(country.cases) }}</p>
          <p class="popup-recovered">Recovered: {{ formatDigits(country.recovered) }}</p>
          <p class="popup-info">Deaths: {{ formatDigits(country.deaths) }}</p>
        </l-popup>
      </l-circle>
    </l-map>
  </div>
</template>
<script>
// Import leaflet components to show map with circle markers
import {
  LMap,
  LTileLayer,
  LCircle,
  LControlLayers,
  LPopup
} from "@vue-leaflet/vue-leaflet";

// Import default css for the
import "leaflet/dist/leaflet.css";

export default {
  components: {
    LMap,
    LTileLayer,
    LCircle,
    LControlLayers,
    LPopup
  },
  props:{
    setMapCenter: Array,
    setMapZoom: Number,
    countries: String,
    selectedType: String
  },
  data() {
    return {
      circleCases: {
        color: '#CC1034',
        multiplayer: 500,
      },
      circleRecovered:{
        color: '#7dd71d',
        multiplayer: 900
      },
      circleDeaths:{
        color: '#fb4443',
        multiplayer: 1800
      }
    };
  },
  methods:{
    setRadius(country, type){
      if(type === "cases"){
        return Math.sqrt(JSON.stringify(country.cases)) * this.circleCases.multiplayer / 4
      }
      else if(type === "recovered"){
        return Math.sqrt(JSON.stringify(country.recovered)) * this.circleRecovered.multiplayer / 3
      }
      else if(type === "deaths"){
        return Math.sqrt(JSON.stringify(country.deaths)) * this.circleDeaths.multiplayer / 2
      }
    },
    formatDigits(number){
      return new Intl.NumberFormat('en-US').format(number);
    },
    handleInfoBoxChange(type){
      if(type === "cases"){
        return this.circleCases.color
      }
      else if(type === "recovered"){
        return this.circleRecovered.color
      }
      else if(type === "deaths"){
        return this.circleDeaths.color
      }
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

    .popup{
      width: 150px;

      &-flag{
        width: 100%;
        border-radius: 15px;
      }

      &-countryName{
        font-size: 1.5rem;
      }

      &-cases, &-recovered, &-info{
        font-size: 14px;
        margin-top: 5px !important;
        margin-bottom: 5px !important;
      }
    }
</style>
