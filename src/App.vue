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
        :cases='countryInfo.todayCases'
        :total='countryInfo.cases'
        />
        <InfoBox 
        title="Recovered"
        :cases='countryInfo.todayRecovered'
        :total='countryInfo.recovered'
        />
        <InfoBox
        title="Deaths"
        :cases='countryInfo.todayDeaths'
        :total='countryInfo.deaths'
        />
      </div>
      <Map />
    </div>
    <div class="right__content">
      <h1> Live cases by country: </h1>
      <Table :countries='tableData' />
      <h1> Worlwide new cases: </h1>
      <Graph />
    </div>
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
          countryInfo: {},
          tableData: [],
          setTableData: []
      }
  },
  mounted(){
    const url ='https://disease.sh/v3/covid-19/all'
    const url2 = 'https://disease.sh/v3/covid-19/countries'

    axios.get(url2)
      .then(response => {
        return response.data
      })
      .then(data =>{
        this.tableData = data.sort((a,b) => {return b.cases - a.cases})
      })

    axios.get(url)
      .then(response =>{
        return response.data
      })
      .then(data => {
        this.countryInfo = data
      })
  },
  methods:{
    handleCountryChange(event){
      const { value } = event.target;
      this.setCountry = value
    },
    getCovidData(){
      const url = this.setCountry === 'all' ? 'https://disease.sh/v3/covid-19/all' : `https://disease.sh/v3/covid-19/countries/${this.setCountry}`

      axios.get(url)
        .then(response =>{
          return response.data
        })
        .then(data => {
          this.countryInfo = data
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
    font-family: 'Monospace', sans-serif;
  }

  .wrapper{
    padding: 40px;
    display: flex;
    flex-direction: column;
    width: 100%;
    height: auto;

    @media (min-width: 1440px){
      flex-direction: row;
    }

    .left__content{
      width: 100%;

      @media (min-width: 1440px){
        width: 70vw;
      }
    }

    .right__content{
      width: 100%;

      h1{
        padding: 10px 0;
      }

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
