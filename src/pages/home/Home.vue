<template>
  <div>
    <home-header></home-header>
    <home-swiper :list="swiperList"></home-swiper>
    <home-icons :list="iconsList"></home-icons>
    <home-recommend :list="recommendList"></home-recommend>
    <home-weekend :list="weekendList"></home-weekend>
    <div class="video">
      <div class="video-button-next" @click="close">点击关闭/打开音乐</div>
      <div class="audio">
        <!--隐藏audio标签-->
        <audio autoplay="autoplay" id="music1">
          <source src="http://188.131.150.199:8080/static/music/Da-iCE.mp3">
        </audio>
      </div>
    </div>
  </div>
</template>

<script>
import HomeHeader from './components/Header'
import HomeSwiper from './components/Swiper'
import HomeIcons from './components/Icons'
import HomeRecommend from './components/Recommend'
import HomeWeekend from './components/Weekend'
import axios from 'axios'
import {mapState} from 'vuex'

export default{
  name: 'Home',
  components: {
    HomeHeader,
    HomeSwiper,
    HomeIcons,
    HomeRecommend,
    HomeWeekend
  },
  data () {
    return {
      swiperList: [],
      iconsList: [],
      recommendList: [],
      weekendList: [],
      lastCity: ''
    }
  },
  computed: {
    ...mapState(['city'])
  },
  methods: {
    getHomeData () {
      // axios.get('/api/index.json?city=' + this.city)
      //   .then(this.getHomeDataSucc)
      axios.get('/api/index?city=' + this.city)
        .then(this.getHomeDataSucc)
    },
    getHomeDataSucc (res) {
      res = res.data
      if (res.result && res.data) {
        const data = res.data
        this.swiperList = data.swiperList
        this.iconsList = data.iconsList
        this.recommendList = data.recommendList
        this.weekendList = data.weekendList
      }
    },
    close () {
      var audio = document.getElementById('music1')
      if (audio.paused) {
        audio.play()// 播放
      } else {
        audio.pause()// 暂停
      }
    }
  },
  mounted () {
    this.lastCity = this.city
    this.getHomeData()
  },
  activated () {
    if (this.lastCity !== this.city) {
      this.lastCity = this.city
      this.getHomeData()
    }
  }
}
</script>

<style lang="stylus" scoped>
.video
  z-index: 2
  position:fixed
  top: 1rem
  width: 100%
  height: 1rem
  .video-button-next
    height: .6rem
    line-height: .6rem
    color: #00bcd4
    font-size: .48rem
    text-align: center
</style>
