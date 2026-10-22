<template>
  <div>
    <city-header></city-header>
    <div v-if="loadError" class="load-error">数据加载失败，请检查网络后刷新重试</div>
    <city-search :cities="cities"></city-search>
    <city-list :cities="cities" :hot="hotCities" :letter="letter"></city-list>
    <city-alphabet :cities="cities" @change="handleLetterChange"></city-alphabet>
  </div>
</template>

<script>
import axios from 'axios'
import CityHeader from './components/Header'
import CitySearch from './components/Search'
import CityList from './components/List'
import CityAlphabet from './components/Alphabet'

export default {
  name: 'City',
  components: {
    CityHeader,
    CitySearch,
    CityList,
    CityAlphabet
  },
  data () {
    return {
      cities: {},
      hotCities: [],
      letter: '',
      loadError: false
    }
  },
  methods: {
    getCityInfo () {
      this.loadError = false
      axios.get('/static/mock/city.json')
        .then(this.handleGetCityInfoSucc)
        .catch(this.handleGetCityInfoError)
    },
    handleGetCityInfoError () {
      this.loadError = true
    },
    handleGetCityInfoSucc (res) {
      // console.log(res)
      res = res.data
      if (res.ret && res.data) {
        const data = res.data
        this.cities = data.cities
        this.hotCities = data.hotCities
      }
    },
    handleLetterChange (letter) {
      this.letter = letter
    }
  },
  mounted () {
    this.getCityInfo()
  }
}
</script>

<style lang="stylus" scoped>
  .load-error
    padding .3rem 0
    text-align center
    font-size .28rem
    color #999
</style>
