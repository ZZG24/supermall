<template>
  <div id="app">
    <router-view/>
    <MainTabbar/>
  </div>
</template>

<script>
  import MainTabbar from './components/content/mainTabbar/MainTabBar'

  export default {
    name: 'App',
    components: {
        MainTabbar
    }
  }
</script>

<style scoped>
  @import "./assets/css/base.css";
</style>
