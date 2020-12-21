<template>
  <Header
    @handleCountryChange="handleCountryChange"
  />
  <h1>{{ setCountry }}</h1>
</template>

<script>
import  Header  from './components/header/header.vue'

export default {
  name: 'App',
  components:{
    Header
  },
  data: function(){
      return {
          setCountry: 'all',
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
</style>
