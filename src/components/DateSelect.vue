<script>
import moment from 'moment'

export default{
  name: 'DateSelect',
  props: ['country', 'dataDates','currentDateSelected'],
  data() {
    return {
      selected: this.currentDateSelected
    }
  },
  methods: {
    onChange() {
      console.log("LAST Date ITEM: ",this.dataDates.slice(-1)[0])
      // TODO: pass specific date to emitter "getDateData"
      // that will update "stats" for component DataBoxes
      // 
      //filter the date that matches
      const statsDate = this.dataDates.find((date) => {
       console.log("DATEPING",date === this.selected)
       if(date === this.selected){
        console.log("DATE:",date)
        return date
       }
      })

      //console.log("DATEs DROPDOWN: ",stat.date)

      this.$emit('get-date', statsDate,this.selected)
    },
    
  },
  //so all dates for provided country
  computed: {
    timestamp: function(){
      console.log("SELECTED date", this.dataDates)
      return moment(this.dataDates).format('MMMM Do YYYY, h:mm:ss a')
    },
    countryDates: function(){
      console.log("COUNTRY", this.country)

      return "ping";
    }
  }
}
</script>

<template>
  <div class="text-center">
    <h2 class="text-3xl font-bold">{{ country }}</h2>
    <div class="text-2xl mt-4 mb-10">
      <!-- TODO: display timestamp  -->
      <!-- {{ timestamp }} -->
      <div>
        <select
          @change="onChange()"
          v-model="selected"
          class="text-black form-select mt-1 block w-full border p-3 rounded"
        >
          <option value="0">Select Date</option>
          <option v-for="date in dataDates" :key="date" :value="date">
            {{ date }}
          </option>
        </select>
      </div>
    </div>
  </div>
</template>

