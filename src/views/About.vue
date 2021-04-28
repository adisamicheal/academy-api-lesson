<template>
  <div>
    <button class="btn btn-primary" @click="filterLocation">
      filter by location
    </button>
    <div class="nn">

      {{ name | capitalize }}
    </div>
    <div class="container p-5" v-if="cases && cases.length != 0">
      <div class="row">
        <div class=" col-3 card m-3" style="width: 18rem;" v-for="caseVal in cases" :key="caseVal.id">
          <div class="card-body">
            <h5 class="card-title"> Country: {{ caseVal.All.country | capitalize }}</h5>
            <h6 class="card-subtitle mb-2 text-muted">Capital City: {{ caseVal.All.capital_city }}</h6>
            <h6 class="card-subtitle mb-2 text-muted">Cases Confirmed: {{ caseVal.All.confrimed }}</h6>
            <h6 class="card-subtitle mb-2 text-muted">Continent: {{ caseVal.All.continent }}</h6>
            <h6 class="card-subtitle mb-2 text-muted">Location: {{ caseVal.All.location }}</h6>
            <h6 class="card-subtitle mb-2 text-muted">Deaths: {{ caseVal.All.deaths }}</h6>
          </div>
      </div>
      </div>
    </div>
    <div v-else>
      Loading...
    </div>
  </div>
</template>

https://covid-api.mmediagroup.fr/v1/cases

<script>
import axios from 'axios'

export default {
  name: 'About',
  data() {
    return {
      cases: null,
      name: 'Micheal'
      // casesAll: null
    }
  },
  mounted() {
    this.getCovidData()
  },
  filters:{
    capitalize(value) {
      return value && value.toUpperCase()
    }
  },
  methods: {
    getCovidData() {
      axios.get('https://covid-api.mmediagroup.fr/v1/cases')
      .then((res) => 
        this.cases = res.data,
        // this.casesAll = res.data.All
        )
      .catch((err) => console.log(err))
    },
    filterLocation() {
      const filtered = this.cases && this.cases.filter(data => (data.All.location === "West Africa"))
      console.log(filtered);
    }
  },
  computed: {
    filteredData() {
      return this.cases && this.cases.filter( ({ All }) => (All.location === "West Africa"))
    }
  },
}
</script>

<style>

</style>
