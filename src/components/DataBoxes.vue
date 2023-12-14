<template>
  <div class="grid md:grid-cols-2 gap-4">
    <!-- Box 1 CASES-->
    <div class="shadow-md bg-blue-100 p-10 text-center rounded">
      <h3 class="text-3xl text-blue-900 font-bold mb-4">Cases</h3>

      <div class="text-2xl mb-4 text-black">
        <span class="font-bold text-black">Confirmed:</span>
        <!-- {{ numberWithCommas(stats.TotalConfirmed) }} -->
        {{ stats.confirmed }}
      </div>
    </div>

    <!-- Box 2: DEATHS -->
    <div class="shadow-md bg-blue-200 p-10 text-center rounded">
      <h3 class="text-3xl text-blue-900 font-bold mb-4">Deaths</h3>
      <div class="text-2xl mb-4 text-black">
        <span class="font-bold text-black">Total:</span>
        <!-- show deaths for 2020-1-22 -->
        {{ stats.deaths }}               
      </div>
    </div>
  </div>
</template>

<script>
export default{
  // on create process date data passed from DateSelect
  // calculate total deaths
  name: 'DataBoxes',
  props: ['stats'],
  methods: {
    numberWithCommas(x){
        return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    }
  }
}
</script>
