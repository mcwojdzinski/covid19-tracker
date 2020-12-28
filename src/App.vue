<template>
  <div class="wrapper">
    <div class="left__content">
      <Header
        @handleCountryChange="handleCountryChange"
        @change="getCovidData"
      />
      <div class="card__wrapper">
        <InfoBox 
        title="Coronavirus Cases"
        :cases='setCountryJson.todayCases'
        :total='setCountryJson.cases'
        />
        <InfoBox 
        title="Recovered"
        :cases='setCountryJson.recovered'
        :total='setCountryJson.todayRecovered'
        />
        <InfoBox
        title="Deaths"
        :cases='setCountryJson.deaths'
        :total='setCountryJson.todayDeaths'
        />
      </div>
      <Map />
      <h1>{{ setCountry }}</h1>
    </div>
    <div class="right__content">
      <h1> Live cases by country: </h1>
      <Table />
      <h1> Worlwide new cases: </h1>
      <Graph />
    </div>
  </div>
  <div>
    {{ setCountryJson }}
  </div>
</template>

<script>
// Import axios for fetching data for covid-19
import axios from 'axios'

// Import UI components
import  Header  from './components/Header/Header.vue'
import InfoBox from './components/InfoBox/InfoBox.vue'
import Map from './components/Map/Map.vue'
import Table from './components/Table/Table.vue'
import Graph from './components/Graph/Graph.vue'

export default {
  name: 'App',
  components:{
    Header,
    InfoBox,
    Map,
    Table,
    Graph
  },
  data: function(){
      return {
          setCountry: 'all',
          setCountryJson: {}
      }
  },
  mounted(){
    const url ='https://disease.sh/v3/covid-19/all'

    axios.get(url)
      .then(response =>{
        return response.data
      })
      .then(data => {
        this.setCountryJson = data
      })
  },
  methods:{
    handleCountryChange(event){
      const { value } = event.target;
      this.setCountry = value
    },
    getCovidData(){
      const url = this.setCountry === 'all' ? 'https://disease.sh/v3/covid-19/all' : `https://disease.sh/v3/covid-19/countries/${this.setCountry}`
      console.log(url)

      axios.get(url)
        .then(response =>{
          return response.data
        })
        .then(data => {
          this.setCountryJson = data
        })
    }
  }
}
</script>

<style lang="scss">
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .wrapper{
    display: flex;
    flex-direction: column;

    @media (min-width: 1440px){
      flex-direction: row;
    }

    .left__content{
      width: 100vw;

      @media (min-width: 1440px){
        width: 70vw;
      }
    }

    .right__content{
      width: 100vw;

      @media (min-width: 1440px){
        width: 30vw;
      }
    }
  }

  .card__wrapper{
    display: flex;
    flex-direction: row;
  }
</style>
