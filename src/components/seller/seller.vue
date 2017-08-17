<template>
  <div class="seller">
    <div class="seller-box" ref="sellerWrap">
      <div class="seller-wrap" >
        <div class="title-wrap">

          <div class="top border-1px">
            <div class="left">
              <h3 class="title">{{seller.name}}</h3>
              <div class="rating">
                <star-group :score="seller.score"></star-group>
                <span class="count">({{seller.ratingCount}})</span>
                <span class="sell-count">月售{{seller.sellCount}}单</span>
              </div>
            </div>

            <div class="right">
              <div class="collection" @click="collectFood">
                <div :class="collectionClass"></div>
                <div class="text" v-show="!isCollect">收藏</div>
                <div class="text" v-show="isCollect">已收藏</div>
              </div>
            </div>
          </div>

          <div class="bottom">
            <div class="detail">
              <p class="text">起送价</p>
              <p class="price">{{seller.minPrice}}元</p>
            </div>

            <div class="detail">
              <p class="text">商家配送</p>
              <p class="price">{{seller.deliveryPrice}}元</p>
            </div>

            <div class="detail">
              <p class="text">平均配送时间</p>
              <p class="price">{{seller.deliveryTime}}分钟</p>
            </div>
          </div>

        </div>

        <split-bar></split-bar>

        <div class="notice-activity">
          <h3 class="title">公告与活动</h3>
          <p class="notice">{{seller.bulletin}}</p>
          <div class="sell-discount">
            <i-support key="noticeSupport" :support="item"  v-for="(item, index) in seller.supports"></i-support>
          </div>
        </div>

        <split-bar></split-bar>

        <div class="real-view">
          <h3 class="title">商家实景</h3>
          <div class="img-wrap" ref="imgWrap">
            <ul class="img-box" >
              <li class="img-pic" v-for="pic in seller.pics">
                <img :src="pic" alt="" width="120" height="90">
              </li>
            </ul>
          </div>
        </div>

        <split-bar></split-bar>

        <div class="seller-infomation">
          <h3 class="title">商家信息</h3>
          <p class="info" v-for="info in seller.infos">
            {{info}}
          </p>
        </div>

      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import starGroup from '../star-group/star-group'
  import support from '../support/support'
  import splitBar from '../split-bar/split-bar'
  import shopcart from '../shopcart/shopcart'

  export default {
    components: {
      starGroup,
      'i-support': support,
      splitBar,
      shopcart
    },
    name: 'seller',
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        isCollect: false
      }
    },
    methods: {
      collectFood () {
        this.isCollect = !this.isCollect
      },
      initScroll () {
        this.sellerScroll = new BScroll(this.$refs.sellerWrap, {
          probeType: 3,
          click: true
        })
        this.imgScroll = new BScroll(this.$refs.imgWrap, {
          scrollX: 'true'
        })
      }
    },
    mounted () {
      this.initScroll()
    },
    computed: {
      collectionClass () {
        return [
          `icon`,
          `icon-favorite`,
          {
            'active': this.isCollect
          }
        ]
      }
    }
  }
</script>

<style>
    @import "./seller.css";
</style>
