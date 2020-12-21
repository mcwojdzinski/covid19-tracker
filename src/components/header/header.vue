<template>
    <div class="app__header">
        <h1> COVID-19 Tracker </h1>
        <select
            v-model="selectedCountry"
            @input="(event) => this.$emit('handleCountryChange', event)">
            <option
                :key="country.name"
                v-for="country in countries"
                :value="country.code"
            >
            {{ country.name }}
            </option>
        </select>
    </div>
</template>
<script>
import axios from 'axios'

export default {
  name: 'Header',
  data: function(){
      return{
          countries: [{
              name: "Worldwide",
              code: "all"
          }],
          selectedCountry: 'all'
      }
  },
  methods:{
  },
  mounted(){
      axios.get("https://disease.sh/v3/covid-19/countries")
        .then(response =>{
          return response.data
        })
        .then(data => {
        
          this.countries = this.countries.concat(
            data.map((country) =>{
                return {
                name: country.country,
                code: country.countryInfo.iso2
                }
            })
          )
        })
  }
}
</script>
<style lang="scss">
    .app__header{
        display: flex;
        flex-direction: row;
        align-items: center;
        margin-bottom: 20px;
        justify-content: space-between;
    }
</style>