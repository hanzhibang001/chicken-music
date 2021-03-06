<template>
    <div class="music-list" ref="client">
        <div class="back" @click="_back">
            <i class="icon-back"></i>
        </div>
        <scroll :data="songs" @scroll="scroll" :listen-scroll="listenScroll" :probe-type="probeType" class="list">
            <div>
                <h1 class="title" v-html="title"></h1>
                <div class="bg-image" :style="image" ref="bgImage">
                    <div class="play-wrapper" v-show="songs.length > 0">
                        <div class="play">
                            <i class="icon-play"></i>
                            <span class="text">随机播放全部</span>
                        </div>
                    </div>
                    <div class="filter" ref="filter"></div>
                </div>
                <div class="song-list-wrapper">
                    <song-list @select="selectItem" :songs="songs"></song-list>
                </div>
            </div>
        </scroll>
        <div class="loading-wrapper" :style="top" v-show="!songs.length">
            <div class="loading-container">
                <loading></loading>
            </div>
        </div>
    </div>
</template>

<script>
import Scroll from 'base/scroll/scroll'
import SongList from 'base/song-list/song-list'
import Loading from 'base/loading/loading'
import {mapActions} from 'vuex'

export default{
    props: {
        bgImage: {
            type: String,
            default: ''
        },
        songs: {
            type: Array,
            default: []
        },
        title: {
            type: String,
            default: ''
        }
    },
    data() {
        return {
            scrollY: 0,
            imageHeight: 0,
            clientHeight: 0
        }
    },
    computed: {
        image() {
            return `background-image:url(${this.bgImage})`
        },
        top() {
            return `top:${this.imageHeight}px;height:${this.clientHeight}px`
        }
    },
    created() {
      this.probeType = 3
      this.listenScroll = true
    },
    mounted() {
      this.imageHeight = this.$refs.bgImage.clientHeight
      this.clientHeight = this.$refs.client.clientHeight - this.imageHeight
    },
    methods: {
        _back() {
            this.$router.back()
        },
        scroll(pos) {
            this.scrollY = pos.y
        },
        selectItem(item, index) {
            this.selectPlay({
                list: this.songs,
                index: index
            })
        },
        ...mapActions([
            'selectPlay'
        ])
    },
    components: {
        SongList,
        Scroll,
        Loading
    }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"

  .music-list
    position: fixed
    z-index: 100
    top: 0
    left: 0
    bottom: 0
    right: 0
    background: $color-background
    .back
      position absolute
      top: 0
      left: 6px
      z-index: 50
      .icon-back
        display: block
        padding: 10px
        font-size: $font-size-large-x
        color: $color-theme
    .title
      position: absolute
      top: 0
      left: 10%
      z-index: 40
      width: 80%
      no-wrap()
      text-align: center
      line-height: 40px
      font-size: $font-size-large
      color: $color-text
    .bg-image
      position: relative
      width: 100%
      height: 0
      padding-top: 70%
      transform-origin: top
      background-size: cover
      .play-wrapper
        position: absolute
        bottom: 20px
        z-index: 50
        width: 100%
        .play
          box-sizing: border-box
          width: 135px
          padding: 7px 0
          margin: 0 auto
          text-align: center
          border: 1px solid $color-theme
          color: $color-theme
          border-radius: 100px
          font-size: 0
          .icon-play
            display: inline-block
            margin-right: 6px
            font-size: $font-size-medium-x
            vertical-align: middle
          .text
            display: inline-block
            vertical-align: middle
            font-size: $font-size-small
      .filter
        position: absolute
        top: 0
        left: 0
        width: 100%
        height: 100%
        background: rgba(7, 17, 27, 0.4)
    .list
      position: absolute
      top: 0
      bottom: 0
      width: 100%
      background: $color-background
      .song-list-wrapper
        padding: 20px 30px
    .loading-wrapper
        position: relative
        top: 0
        bottom: 0
        width: 100%
        height: 0
        .loading-container
          position: absolute
          width: 100%
          top:50%
          transform: translateY(-50%)
</style>