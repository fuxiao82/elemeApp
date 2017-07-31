<template>
  <div class="header">
    <div class="seller">
      <div class="picture">
        <img :src="seller.avatar" alt="avatar">
      </div>
      <div class="seller-info">
        <div class="name with-icon">
          {{seller.name}}
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support">
          <div :class="['supports', supportType[item.type]]" v-for="(item, index) in seller.supports"
               :type="item.type" v-if="index === 0">
            {{item.description}}
          </div>
          <div class="support-fold" @click="showDetail">
            {{seller.supports ? seller.supports.length : 0}}个
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

          <div class="support">
            <div class="titles">
              <div class="split-line"></div>
                优惠信息
              <div class="split-line"></div>
            </div>
            <div class="content">
              <div :class="['supports', supportType[item.type]]" v-for="(item, index) in seller.supports"
                   :type="item.type">
                {{item.description}}
              </div>
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
  export default {
    name: 'header',
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
