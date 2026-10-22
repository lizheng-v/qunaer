<template>
<div>
  <detail-banner
    :sightName="sightName"
    :bannerImg="bannerImg"
    :bannerImgs="gallaryImgs"
  ></detail-banner>
  <detail-header></detail-header>
  <div v-if="loadError" class="load-error">数据加载失败，请检查网络后刷新重试</div>
  <div class="content">
    <detail-list :list="list"></detail-list>
  </div>
</div>
</template>

<script>
import DetailBanner from './components/Banner'
import DetailHeader from './components/Header'
import DetailList from './components/List'
import axios from 'axios'

export default {
  name: 'Detail',
  components: {
    DetailBanner,
    DetailHeader,
    DetailList
  },
  data () {
    return {
      sightName: '',
      bannerImg: '',
      gallaryImgs: [],
      list: [],
      loadError: false
    }
  },
  methods: {
    getDetailInfo () {
      this.loadError = false
      axios.get('/static/mock/detail.json', {
        params: {
          id: this.$route.params.id
        }
      }).then(this.handleGetDataSucc)
        .catch(this.handleGetDataError)
    },
    handleGetDataError () {
      this.loadError = true
    },
    handleGetDataSucc (res) {
      res = res.data
      if (res.ret && res.data) {
        const data = res.data
        this.sightName = data.sightName
        this.bannerImg = data.bannerImg
        this.gallaryImgs = data.gallaryImgs
        this.list = data.categoryList
      }
    }
  },
  mounted () {
    this.getDetailInfo()
  }
}
</script>

<style lang="stylus" scoped>
  .content
    height 50rem
  .load-error
    padding .3rem 0
    text-align center
    font-size .28rem
    color #999
</style>
