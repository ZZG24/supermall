<template>
  <div class="wrapper" ref="wrapper">
      <div>
          <slot></slot>
      </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  
export default {
    props: {
        probeType: {
            type: Number,
            default: 0
        },
        pullUpLoad: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            scroll: null
        }
    },
    mounted() {
        //1.创建BScroll对象
        this.scroll = new BScroll(this.$refs.wrapper, {
            click: true,
            probeType: this.probeType,
            pullUpLoad: this.pullUpLoad

        })
        //2.监听滚动的位置
        if (this.probeType === 2 || this.probeType === 3) {
            this.scroll.on('scroll',(postion) =>{
            this.$emit('scroll',postion)
        })
        }
        //3.监听scroll滚动到底部
        if (this.pullUpLoad) {
            this.scroll.on('pullingUp', () => {
                // console.log('监听');
                this.$emit('pullingUp')
                
            })
        }
    },
    methods: {
        scrollTo(x,y,time=300) {
            this.scroll.scrollTo(x,y,time)
        },
        refresh() {
            this.scroll && this.scroll.refresh()
        },
        finishPullUp() {
            this.scroll && this.scroll.finishPullUp() 
        }
        
    }
    
}
</script>
<style>

</style>