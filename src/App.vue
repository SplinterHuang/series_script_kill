<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Index></Index>
  </div>
</template>

<script>
import Index from './pages/index'

export default {
  name: 'App',
  created() {
    document.title = '瑾生梦红楼'
  },
  components: {
    Index,
  }
}
</script>

<style>

</style>
