<template>
  <div class="wrapper">
    <div class="left__content">
      <Header
        @handleCountryChange="handleCountryChange"
        @change="getCovidData"
      />
      <div class="card__wrapper">
        <InfoBox 
        @click="this.selectedType = 'cases'"
        :active="this.selectedType === 'cases'"
        :type="this.selectedType"
        title="Coronavirus Cases"
        :cases='formatDigits(countryInfo.todayCases)'
        :total='formatDigits(countryInfo.cases)'
        />
        <InfoBox 
        @click="this.selectedType = 'recovered'"
        :active="this.selectedType === 'recovered'"
        :type="this.selectedType"
        title="Recovered"
        :cases='formatDigits(countryInfo.todayRecovered)'
        :total='formatDigits(countryInfo.recovered)'
        />
        <InfoBox
        @click="this.selectedType = 'deaths'"
        :active="this.selectedType === 'deaths'"
        :type="this.selectedType"
        title="Deaths"
        :cases='formatDigits(countryInfo.todayDeaths)'
        :total='formatDigits(countryInfo.deaths)'
        />
      </div>
      <Map
        :countries='tableData'
        :setMapCenter='setMapCenter'
        :setMapZoom='setMapZoom'
        :selectedType = 'selectedType'
      />
    </div>
    <div class="right__content">
      <h1> Live cases by country: </h1>
      <Table :countries='tableData' />
    </div>
  </div>
</template>

<script>
// Import axios for fetching data for covid-19
import axios from 'axios'

// Import UI components
import Header from './components/Header/Header.vue'
import InfoBox from './components/InfoBox/InfoBox.vue'
import Map from './components/Map/Map.vue'
import Table from './components/Table/Table.vue'

export default {
  name: 'App',
  components:{
    Header,
    InfoBox,
    Map,
    Table
  },
  data: function(){
      return {
          setCountry: 'all',
          countryInfo: {},
          tableData: [],
          selectedType: "cases",
          setMapCenter: [30,12],
          setMapZoom: 2
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
          if(this.setCountry === 'all'){
            this.setMapCenter = [30,12]
            this.setMapZoom = 2
          } else {
            this.setMapCenter = [data.countryInfo.lat, data.countryInfo.long]
            this.setMapZoom = 5
          }
        })
    },
    formatDigits(number){
      return new Intl.NumberFormat('en-US').format(number);
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
    height: 100vh;

    @media (min-width: 1440px){
      flex-direction: row;
    }

    .left__content{
      width: 100%;
      padding: 20px;

      @media (min-width: 1440px){
        width: 70vw;
      }
    }

    .right__content{
      width: 100%;
      padding: 20px;

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

  .cases{
    border-bottom: 10px;
  }
</style>
