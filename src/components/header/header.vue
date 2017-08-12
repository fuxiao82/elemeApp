<template>
  <div class="header">
    <div class="seller">
      <div class="picture">
        <img :src="seller.avatar" alt="avatar">
      </div>
      <div class="seller-info">
        <div class="name">
          <div class="icon"></div>
          <p>{{seller.name}}</p>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support-fold" v-if="seller.supports">
          <i-support :support="seller.supports[0]" key="headerSupport" :isWhite="true" type="hideborder"></i-support>
          <div class="fold-icon" @click="showDetail">
            {{seller.supports.length}}个
          </div>
        </div>
      </div>
    </div>
    <div class="bulletin">
      <p class="bulletin-text" @click="showDetail">{{seller.bulletin}}</p>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="background">
    </div>

    <div class="detail" v-show="isShowDetail">
      <div class="content-wrap">
        <div class="content-main">

          <div class="title">{{seller.name}}</div>

          <div class="stars">
            <div v-for='(item, index) in STAR_COUNT'
                 :class="['star',
             {'star-on': index < Math.ceil(seller.rankRate/20) &&
             !rankRateHalf || index < Math.ceil(seller.rankRate/20) - 1 && rankRateHalf,
             'star-half': rankRateHalf && index === Math.ceil(seller.rankRate/20) - 1}]">
            </div>
          </div>

          <div class="support-group">
            <div class="titles">
              <div class="split-line"></div>
                优惠信息
              <div class="split-line"></div>
            </div>
            <div class="content">
              <i-support key="headerContentSupport" :support="support" :isWhite="true" v-for="support in seller.supports"></i-support>
            </div>
          </div>

          <div class="bulletins">
            <div class="titles">
              <div class="split-line"></div>
                商家公告
              <div class="split-line"></div>
            </div>
            <div class="content">
              <p class="bulletin-text">{{seller.bulletin}}</p>
            </div>
          </div>

        </div>
      </div>

      <div class="close-wrap" @click="hideDetail">×</div>

    </div>

  </div>
</template>


<script type="text/ecmascript-6">
  import support from '../support/support'

  export default {
    name: 'header',
    components: {
      'i-support': support
    },
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        /*
         * 优惠信息类型
         */
        supportType: ['decrease', 'discount', 'guarantee', 'invoice', 'special'],
        isShowDetail: false,
        STAR_COUNT: 5,
        rankRateHalf: !(parseFloat(this.seller.rankRate) % 10 === 0)   // 是否有半颗星
      }
    },
    methods: {
      showDetail: function () {
        this.isShowDetail = true
      },
      hideDetail: function () {
        this.isShowDetail = false
      }
    },
    mounted: function () {
    }
  }
</script>

<style>
  @import "header.css";

</style>
