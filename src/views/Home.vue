<template>

  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />

      <div class="flex flex-col items-center justify-center mb-10">

         <div class="flex items-center justify-center w-full">
            <CountrySelect class="mr-6" @get-country="getCountryData" :countries="countries" />
      
          <button 
            @click="clearCountry"
            v-if="stats.Country" 
            class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
            Clear 
          </button>

          </div>

          <button @click="showCharts" class="bg-blue-300 rounded my-10 focus:outline-none hover:bg-blue-400 p-3 ">View Charts</button> 

          <Charts :data="allData" v-if="showchart" />

      </div>


  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <img :src="loadingImage" class="w-24 m-auto" alt="">
  </main>

</template>

<script>

import DataTitle from '@/components/DataTitle'
import DataBoxes from '@/components/DataBoxes'
import CountrySelect from '@/components/CountrySelect'
import Charts from '@/components/Charts'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    Charts
  },
  data() {
    return {
      allData: {},
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/hourglass.gif'),
      showchart: false
    }
  },
  methods: {
    async fetchCoivdData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },

    getCountryData(country) {
      this.stats = country
      this.title = country.Country
    },

    async clearCountry(){
      this.loading = true
      const data = await this.fetchCoivdData()
      this.title = 'Global'
      this.stats = data.Global
      this.loading = false
    },

    showCharts(){
      this.showchart = !this.showchart
    }
  },
  async created() {
    const data = await this.fetchCoivdData()

    this.allData = data
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  }
}
</script>
