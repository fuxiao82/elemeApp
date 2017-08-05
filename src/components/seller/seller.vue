<template>
  <div class="seller">
    <div class="title-wrap">

      <div class="top">
        <div class="left">
          <h3 class="title">{{seller.name}}</h3>
          <div class="rating">
            <star-group :score="seller.score"></star-group>
            <span class="count">({{seller.ratingCount}})</span>
            <span class="sell-count">月售{{seller.sellCount}}单</span>
          </div>
        </div>

        <div class="right">
          <div class="collection">
            <div class="icon"></div>
            <div class="text">收藏</div>
            <div class="text">已收藏</div>
          </div>
        </div>
      </div>

      <div class="bottom">
        <div class="start-price">
          <p class="text">起送价</p>
          <p class="price">{{seller.minPrice}}元</p>
        </div>

        <div class="send-price">
          <p class="text">商家配送</p>
          <p class="price">{{seller.deliveryPrice}}元</p>
        </div>

        <div class="send-time">
          <p class="text">平均配送时间</p>
          <p class="price">{{seller.deliveryTime}}分钟</p>
        </div>
      </div>

    </div>

    <div class="notice-activity">
      <h3>公告与活动</h3>
      <p class="notice">{{seller.bulletin}}</p>
      <div class="sell-discount" v-for="support in seller.supports">
        <support :support="support"></support>
      </div>
    </div>

    <div class="real-view">
      <h3 class="title">商家实景</h3>
      <img :src="pic" alt="" v-for="pic in seller.pics">
    </div>

    <div class="seller-info">
      <h3 class="title">商家信息</h3>
      <p class="info" v-for="info in seller.infos">
        {{info}}
      </p>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import starGroup from '../star-group/star-group'
  import support from '../support/support'

  export default {
    components: {
      starGroup,
      support
    },
    name: 'seller',
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
      }
    },
    created () {
      console.log(this.seller)
    }
  }
</script>

<style>
    @import "./seller.css";
</style>
