<template>
  <div class="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="goods tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="ratings tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="seller tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
    <div class="content"></div>
  </div>
</template>

<script>
import header from './components/header/header.vue'

export default {
  name: 'app',
  data () {
    return {
      seller: {}
    }
  },
  components: {
    'v-header': header
  },
  methods: {
    viewFoodDetail () {
      console.log('viewFoodDetail')
    }
  },
  created () {
    this.$http.get('/api/seller').then(response => {
      this.seller = response.body.data
      console.log(this.seller)
    })
  }
}
</script>

<style>
  @import "./assets/index.css";
</style>
