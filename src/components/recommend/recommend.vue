<template>
  <div class="recommend" ref="recommend">
    <!--scroll组件-->
    <scroll ref="scroll" class="recommend-content" :data="discList">
      <div>
        <!--首先判断是否有数据-->
        <div v-if="recommends.length" class="slider-wrapper" ref="sliderWrapper">
          <slider>
            <div v-for="item in recommends">
              <a :href="item.linkUrl">
                <!--添加class="needsclick"可以 避免与fastclick插件冲突，如果不加，图片不能点击-->
                <img class="needsclick" @load="loadImage" :src="item.picUrl">
              </a>
            </div>
          </slider>
        </div>
        <!--歌单列表-->
        <div class="recommend-list">
          <h1 class="list-title">热门歌单推荐</h1>
          <ul>
            <li v-for="item in discList" class="item">
              <div class="icon">
                <!--图片懒加载中-->
                <img width="60" height="60"  v-lazy="item.imgurl">
              </div>
              <div class="text">
                <h2 class="name" v-html="item.creator.name"></h2>
                <p class="desc" v-html="item.dissname"></p>
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div class="loading-container" v-show="!discList.length">
        <loading></loading>
      </div>
    </scroll>
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import Slider from 'base/slider/slider'
  import Loading from 'base/loading/loading'
  import Scroll from 'base/scroll/scroll'
  import {getRecommend, getDiscList} from 'api/recommend'
  import {ERR_OK} from 'api/config'

  export default {
    data() {
      return {
        recommends: [],
        discList: []
      }
    },
    created() {
      this._getRecommend()

      this._getDiscList()
    },
    methods: {
      loadImage() {
        // 图片加载出来刷新scroll，因为轮播组件的高度由图片撑开
        if (!this.checkloaded) {
          this.checkloaded = true
          // 调用子组件scroll里面的refresh()方法
          this.$refs.scroll.refresh()
        }
      },
      _getRecommend() {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider
          }
        })
      },
      _getDiscList() {
        getDiscList().then((res) => {
          if (res.code === ERR_OK) {
            this.discList = res.data.list
          }
        })
      }
    },
    components: {
      Slider,
      Loading,
      Scroll
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"

  .recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0
    .recommend-content
      height: 100%
      overflow: hidden
      .slider-wrapper
        position: relative
        width: 100%
        overflow: hidden
      .recommend-list
        .list-title
          height: 65px
          line-height: 65px
          text-align: center
          font-size: $font-size-medium
          color: $color-theme
        .item
          display: flex
          box-sizing: border-box
          align-items: center
          padding: 0 20px 20px 20px
          .icon
            flex: 0 0 60px
            width: 60px
            padding-right: 20px
          .text
            display: flex
            flex-direction: column
            justify-content: center
            flex: 1
            line-height: 20px
            overflow: hidden
            font-size: $font-size-medium
            .name
              margin-bottom: 10px
              color: $color-text
            .desc
              color: $color-text-d
      .loading-container
        position: absolute
        width: 100%
        top: 50%
        transform: translateY(-50%)
</style>
