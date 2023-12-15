<script>
import DateSelect from '../components/DateSelect.vue'
import DataBoxes from '../components/DataBoxes.vue'
import CountrySelect from '../components/CountrySelect.vue'


export default {
  name: 'HomeView',
  components: {
    DateSelect,
    DataBoxes,
    CountrySelect
  },
  data() {
    return {
      loading: true,
      country: 'US',
      data:[],
      currentDateSelected:'',
      dataDates: '',
      stats: [],
      countries: [],
      loadingImage: null
    }
  },
  methods: {
    async fetchCovidData() {
      try {
        const response = await fetch('https://pomber.github.io/covid19/timeseries.json')
        const data = await response.json()
        return data
      } catch (error) {
        console.error('Error fetching COVID data:', error)
        throw error // Re-throw the error for handling in the calling code, if needed
      }
    },
    async clearCountryData(){
      this.loading = true
      this.data = await this.fetchCovidData()
      this.country = 'US'
      this.stats = this.data[this.country].slice(-1)[0] //return last element and pull it out
      this.loading = false
    },
    getCountryData(country) {
      console.log('getCountryData: ', country)
      //change the country value
      this.country = country
      //pull out dates list, based on new country
      this.dataDates = this.data[this.country].map(({date})=> date)
      console.log('NEW country DATES', this.dataDates)
      //Update new stats based on new country AND current date
      console.log("SELECTED DATE", this.currentDateSelected)
      this.stats = this.data[this.country].find((item)=> item.date === this.currentDateSelected)
      console.log("WHAT STATS?:", this.stats)
    },
    getDateData(statsDate,selected) {
      //find object with matching date
      let myObject = this.data[this.country].find((item)=>{
        if(item.date === statsDate){
          return item
        }
      })
      this.stats = myObject
      console.log("Specific country date info:", this.stats)

      this.currentDateSelected = selected
    },
 
  },
  async created() {
    const data = await this.fetchCovidData()
    this.data = data;
    console.log('Component is created!', data)

    //set initial dropdown dates to US
    this.dataDates = data[this.country].map(({date})=> date)
    console.log('DATADATES', this.dataDates)

    //set default date
    this.currentDateSelected = this.dataDates.slice(-1)[0]

    //show last confirmed death stats
    this.stats = data[this.country].slice(-1)[0]
    console.log('STATS is US: ', this.stats)

    //pull out specific country names
    this.countries = Object.keys(data)
    console.log('Countries:', this.countries)

    this.loading = false
    // Now, resolve the dynamic import and set the URL to loadingImage
    this.loadingImage = (await import('../assets/hourglass.gif')).default
  },

}
</script>

<template>
  <main v-if="!loading">
    <!-- select a specific country  -->
    <CountrySelect @update="updateDate" 
    @get-country="getCountryData" :countries="countries" />
    <!-- filter country name  -->
    <!-- TODO: start at last date info -->
    <DateSelect @get-date="getDateData" 
    :country="country" :dataDates="dataDates" :currentDateSelected="currentDateSelected" />
    <!-- object with matching date -->
    <DataBoxes :stats="stats" />

    <button @click="clearCountryData" v-if="country" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
      Clear Country
    </button>
  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching Data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="" />
  </main>
</template>
