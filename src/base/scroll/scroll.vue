<template>
  <div ref="wrapper">
    <slot></slot>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'

  export default {
    props: {
      probeType: {
        type: Number,
        default: 1
      },
      click: {
        type: Boolean,
        default: true
      },
      data: {
        type: Array,
        default: null
      }
    },
    mounted() {
      // 确保dom渲染再初始化scroll
      setTimeout(() => {
        this._initScroll()
      }, 20)
    },
    methods: {
      // 初始化
      _initScroll() {
        if (!this.$refs.wrapper) {
          return
        }
        this.scroll = new BScroll(this.$refs.wrapper, {
          probeType: this.probeType,
          click: this.click
        })
      },
      // enable()启用 better-scroll，默认开启
      enable() {
        this.scroll && this.scroll.enable()
      },
      // disable()禁用 better-scroll
      disable() {
        this.scroll && this.scroll.disable()
      },
      // refresh()强制 scroll 重新计算，当 better-scroll 中的元素发生变化的时候调用此方法。
      refresh() {
        this.scroll && this.scroll.refresh()
      }
    },
    watch: {
      data() {
        setTimeout(() => {
          this.refresh()
        }, 20)
      }
    }
  }

</script>

<style scoped lang="stylus" rel="stylesheet/stylus">

</style>
