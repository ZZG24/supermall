<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

    <scroll class="content"
            ref="scroll"
            :probe-type="3" 
            @scroll="contentScroll"
            :pull-up-load="true"
            @pullingUp="loadMore">
      <home-swiper :banners="banners"/>
      <recommend-view :recommends="recommends"/>
      <feature-view></feature-view>
      <tab-control class="tab-control" 
        :titles="['流行', '新品', '精选']" 
        @tabClick="tabClick"/>
      <goods-list :goods="showGoods"/>
    </scroll>
    <back-top @click.native="backClick" v-show="isShowBackTop"/>
    
  </div>
</template>

<script>
 
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'

  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'
  import GoodsListItem from 'components/content/goods/GoodsListItem'
  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backTop/BackTop'

  import {getHomeMultidata,getHomeGoods} from 'network/home'
  import {debounce} from 'common/utils'
  
export default {
  name: "Home",
  components: {
    HomeSwiper,
    RecommendView,
    FeatureView,
    NavBar,
    TabControl,
    GoodsList,
    GoodsListItem,
    Scroll,
    BackTop
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        'pop': {page: 0, list: []},
        'new': {page: 0, list: []},
        'sell': {page: 0, list: []}
      },
      currentType: 'pop',
      isShowBackTop: false
    }
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list
    }
  },
  created() {
    this.getHomeMultidata()

    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },
  mounted() {
    const refresh = debounce(this.$refs.scroll.refresh, 500)
     //监听item中图片加载完成
    this.$bus.$on('itemImageLoad', () =>{
      // this.$refs.scroll.refresh()    
      refresh()
    })
  },
  methods: {
    // 事件监听的相关方法
    loadMore() {
      // console.log('---');
      this.getHomeGoods(this.currentType)
    },
    tabClick(index) {
      switch(index) {
        case 0:
          this.currentType = 'pop'
          break
        case 1:
          this.currentType = 'new'
          break
        case 2:
          this.currentType = 'sell'
          break
      }     
    },
    backClick() {
      this.$refs.scroll.scrollTo(0,0)
      
    },
    contentScroll(position) {
      this.isShowBackTop = (-position.y) > 1000
      
    },
    // 网络请求相关的方法
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
        // console.log(res);
        
      })
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type,page).then(res => {
      this.goods[type].list.push(...res.data.list)
      this.goods[type].page += 1

      //完成上啦加载更多
      this.$refs.scroll.finishPullUp()
      })
    }
  }
}
</script>

<style scoped>
  #home {
    padding-top: 44px;
		position: relative;
		height: 100vh;
  } 

  .home-nav {
    background-color: var(--color-tint);
    color: white;
    position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 9;
	
  } 

  .tab-control {
    position: sticky;
    top: 44px;
    z-index: 9;
  }

  .content { 
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
</style>
