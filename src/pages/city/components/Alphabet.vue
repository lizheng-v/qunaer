<template>
  <ul class="list">
    <li class="item" v-for="item of letters" :key="item" :ref="item"
        @click="handleLetterClick" @touchstart.prevent="handleTouchStart"
        @touchmove="handleTouchMove" @touchend="handleTouchEnd">
      {{item}}
    </li>
  </ul>
</template>

<script>
export default {
  name: 'CityAlphabet',
  props: {
    cities: Object
  },
  computed: {
    letters () {
      const letters = []
      for (let i in this.cities) {
        letters.push(i)
      }
      return letters
    }
  },
  data () {
    return {
      touchStatus: false,
      itemHeight: 20,
      timer: null
    }
  },
  updated () {
    const firstLetter = this.letters[0]
    const firstItem = firstLetter && this.$refs[firstLetter]
    if (firstItem && firstItem[0]) {
      this.itemHeight = firstItem[0].offsetHeight
    }
  },
  methods: {
    handleLetterClick (e) {
      this.$emit('change', e.target.innerText)
    },
    handleTouchStart () {
      this.touchStatus = true
    },
    handleTouchMove (e) {
      if (this.touchStatus) {
        if (this.timer) {
          clearTimeout(this.timer)
        }
        this.timer = setTimeout(() => {
          const listTop = this.$el.getBoundingClientRect().top
          const touchY = e.touches[0].clientY - listTop
          const index = Math.floor(touchY / this.itemHeight)
          if (index >= 0 && index < this.letters.length) {
            this.$emit('change', this.letters[index])
          }
        }, 10)
      }
    },
    handleTouchEnd () {
      this.touchStatus = false
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import "~styles/varibles.styl"
  .list
    display flex
    flex-direction column
    justify-content center
    position absolute
    top 1.6rem
    right 0
    bottom 0
    width .4rem

    .item
      line-height .4rem
      text-align center
      color $bgColor
</style>
