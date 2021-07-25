<template>
  <div id="app">
    <img src="/mke.svg" alt="milwaukee in text" class="_mke" />
    <h1 class="_main-title">Brewery List</h1>
    <form class="_form" @submit.prevent="submit">
      <div class="_filter-bar">
        <div class="_filter">
          <div class="_filter-title">City</div>
          <select class="_dropdown" name="breweries" id="_breweries" v-model="selectedCity">
            <option value="">All</option>
            <option value="milwaukee">Milwaukee</option>
          </select>
        </div>
        <!-- <div>{{ selectedCity }}</div> -->
        <div class="_filter">
          <div class="_filter-title">Type</div>
          <select class="_dropdown" name="types" id="_types" v-model="selectedType">
            <option value="">All</option>
            <option value="micro">Micro</option>
            <option value="nano">Nano</option>
            <option value="regional">Regional</option>
            <option value="brewpub">Brewpub</option>
            <option value="large">Large</option>
            <option value="planning">Planning</option>
            <option value="bar">Bar</option>
            <option value="contract">Contract</option>
            <option value="proprietor">Proprietor</option>
            <!-- <option value="closed">Closed</option> -->
          </select>
        </div>
        <button type="submit" class="_btn" alt="submit form">Submit</button>
      </div>
      <ul class="_list">
        <li
          class="_brewery"
          v-for="brewery in visibleBreweries"
          :key="brewery.id"
          :visibleBreweries="visibleBreweries"
          :currentPage="currentPage"
        >
          <div v-if="brewery.name" class="_col">
            <div class="_title">{{ brewery.name }}</div>
          </div>
          <div v-if="brewery.brewery_type" class="_col">
            <div class="_info-title">Type of Brewery:</div>
            <div class="_info">{{ brewery.brewery_type | uppercase }}</div>
          </div>
          <div v-if="brewery.street" class="_col">
            <div class="_info-title">Address:</div>
            <div class="_info">{{ brewery.street | uppercase }}</div>
          </div>
          <div v-if="brewery.phone" class="_col">
            <div class="_info-title">Phone Number:</div>
            <div class="_info">{{ brewery.phone | formatPhoneNumber }}</div>
          </div>
          <div v-if="brewery.website_url" class="_col">
            <div class="_info-title">Website:</div>
            <a :href="brewery.website_url" class="_info _link" target="_blank" rel="noopener">{{
              brewery.website_url
            }}</a>
          </div>
        </li>
      </ul>
    </form>
    <Pagination
      v-if="visibleBreweries.length"
      :visibleBreweries="visibleBreweries"
      :currentPage="currentPage"
      :perPage="perPage"
      @page-update="updatePage"
    />
  </div>
</template>

<script>
import Pagination from './components/Pagination.vue'
import { BASE_URL } from './config.js'

export default {
  name: 'App',
  data() {
    return {
      selectedCity: '',
      selectedType: '',
      currentPage: 1,
      perPage: 10,
      visibleBreweries: [],
    }
  },
  components: { Pagination },
  //dynamically creating a new queury to the server, becasuse there's no way to get ALL of the city names since the API only allows up to 50 in one call. So I limited it to a few breweries.
  // add pagination****** (make it set a variable that gets set as a query param) -- page
  // The offset or page of breweries to return.
  methods: {
    async submit() {
      let per_page = this.perPage
      let page = this.currentPage
      let params = { per_page, page }
      if (this.selectedCity !== '') {
        params.by_city = this.selectedCity
      }
      if (this.selectedType !== '') {
        params.by_type = this.selectedType
      }
      const res = await axios.get(BASE_URL, { params })
      this.visibleBreweries = res.data
      console.log(`this.visibleBreweries → `, this.visibleBreweries)
      console.log(`this.currentPage → `, this.currentPage)
      this.types = res.data
      console.log(`clicked → `, 'clicked')
    },
    updatePage(pageNum) {
      this.currentPage = pageNum
      this.submit()
    },
    formatPhoneNumber(phoneNumberString) {
      var cleaned = ('' + phoneNumberString).replace(/\D/g, '')
      var match = cleaned.match(/^(\d{3})(\d{3})(\d{4})$/)
      if (match) {
        return '(' + match[1] + ') ' + match[2] + '-' + match[3]
      }
      return null
    },
  },
  filters: {
    uppercase(e) {
      return e.charAt(0).toUpperCase() + e.slice(1)
    },
    formatPhoneNumber(e) {
      var cleaned = ('' + e).replace(/\D/g, '')
      var match = cleaned.match(/^(\d{3})(\d{3})(\d{4})$/)
      if (match) {
        return '(' + match[1] + ') ' + match[2] + '-' + match[3]
      }
      return null
    },
  },
  computed: {},
}
</script>

<style lang="sass">
body
  background-color: #F2AA4CFF
#app
  font-family: Avenir, Helvetica, Arial, sans-serif
  text-align: center
  margin-top: 60px
  color: #545a60
._mke
  width: 300px
._main-title
  font-size: 62px
  margin: 0
._filter-bar
  display: flex
  justify-content: space-around
  align-items: center
  padding: 0 30vw
  margin-top: 32px
._filter
  display: flex
  &-title
    font-size: 32px
    font-weight: bold
    margin-right: 12px
._btn
  background-color: #545a60
  color: #f8f8ff
  font-size: 16px
  border-radius: 16px
  border: none
  padding: 12px
  margin-left: 6px
  &:hover
    transform: translateY(-3px)
  &:active
    transform: translateY(-1px)
._dropdown
  border-radius: 8px
  padding: 0 6px
  border: none
  background-color: #f8f8ff
  margin-right: 12px
._list
  display: inline-flex
  flex: 1
  flex-direction: column
  flex-wrap: wrap
._brewery
  list-style-type: none
  // border: solid 2px #545a60
  border-radius: 1em
  padding: 1em
  margin-bottom: 2em
  background-color: #f8f8ff
  box-shadow: rgba(0, 0, 0, 0.19) 0px 10px 20px, rgba(0, 0, 0, 0.23) 0px 6px 6px
  &:hover
    transform: scale(1.1)
._title
  font-size: 32px
  font-weight: bold
  margin-bottom: 6px
  color: #00539CFF
._info-title
  font-size: 16px
  margin-bottom: 4px
._info
  font-size: 24px
  font-weight: 500
._col
  display: flex
  flex-direction: column
._link
  color: #F2AA4CFF
  text-decoration: underline
</style>