<template>
  <div class="wrapper">
    <div class="left__content">
      <Header
      @handleCountryChange="handleCountryChange"
      />
      <div class="card__wrapper">
        <InfoBox 
        title="Coronavirus Cases"
        cases=123123
        total=20000
        />
        <InfoBox 
        title="Recovered"
        cases=123123
        total=20000
        />
        <InfoBox
        title="Deaths"
        cases=123123
        total=20000
        />
      </div>
      <Map />
      <h1>{{ setCountry }}</h1>
    </div>
    <div class="right__content">
      <Table />
      <Graph />
    </div>
  </div>
</template>

<script>
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
          TotalCases:'Total Cases'
      }
  },
  methods:{
    handleCountryChange(event){
      const { value } = event.target;
      this.setCountry = value
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
