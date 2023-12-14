<script>
export default{
  name: 'CountrySelect',
  props: ['countries'],
  data() {
    return {
      selected: 0
    }
  },
  methods: {
    onChange() {
      const country = this.countries.find((item) => item === this.selected)
      console.log('NEW COUNTRY from DROPDOWN: ', country)
      this.$emit('get-country', country)
    }
  }
}
</script>

<template>
    Show me the country
<select
  @change="onChange()"
  v-model="selected"
  class="text-black form-select mt-1 block w-full border p-3 rounded"
>
  <option value="0">Select Country</option>
  <option v-for="country in countries" :key="country" :value="country">
    {{ country }}
  </option>
</select>
</template>