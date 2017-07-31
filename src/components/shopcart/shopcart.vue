<template>
  <div class="shopcart">
    <div class="left-wrap">
      <div class="cart-wrap" @click="toggleCartList">
        <div :class="['cart-box',{'active': totalCount>0}]">
          <div class="icon-shopping_cart"></div>
        </div>
        <div class="total-count" v-show="totalCount>0">
          {{totalCount}}
        </div>
      </div>
      <div class="price ellipsis">
        ￥{{account}}
      </div>
      <div class="split-line"></div>
      <div class="des">
        另需配送费￥{{deliverPrice}}元
      </div>
    </div>
    <div :class="['right-wrap',{'active': account >= minPrice}]">
      <div class="send-price">
        <span v-if="account < minPrice">￥{{minPrice}}起送</span>
        <span v-if="account >= minPrice" class="go-to-buy">去结算</span>
      </div>
    </div>
    <div :class="{background: isShowCartList}" @click="hideCartList"></div>

    <transition name="updown">
      <div class="shopcart-list" v-show="isShowCartList">
        <div class="header border-1px">
          <p class="title">购物车</p>
          <div class="clear" @click="clearCartList">清空</div>
        </div>
        <div class="content" ref="cartWrap">
          <ul class="content-container">
            <li class="shopcart-list-line" v-for="food in selectFoods">
              <div class="name">{{food.name}}</div>
              <div class="right-cart-box">
                <div class="price">
                  <span class="icon">￥</span>
                  {{food.price * food.count}}
                </div>
                <carcontrol :food="food"></carcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import carcontrol from '../carcontrol/carcontrol.vue'
  import BScroll from 'better-scroll'

  export default {
    name: 'shopcart',
    components: {
      carcontrol
    },
    methods: {
      toggleCartList () {
        if (this.totalCount) {
          this.isShowCartList = !this.isShowCartList
        }
      },
      clearCartList () {
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
      },
      hideCartList () {
        this.isShowCartList = false
      },
      initScroll () {
//        console.log(this.$refs.cartWrap)
        this.cartScroll = new BScroll(this.$refs.cartWrap, {
          click: true,
          probeType: 3
        })
        this.cartScroll.on('scroll', (pos) => {
//          console.log(pos)
        })
      }
    },
    props: {
      total: {
        type: Number
      },
      minPrice: {
        type: Number
      },
      deliverPrice: {
        type: Number
      },
      selectFoods: {
        type: Array,
        default () {
          return []
        }
      }
    },
    data () {
      return {
        isShowCartList: false
      }
    },
    watch: {
      isShowCartList (val) {
//        console.log('change' + this.isShowCartList)
        if (val) {
          this.$nextTick(() => {
            if (!this.cartScroll) {
              this.initScroll()
            } else {
              this.cartScroll.refresh()
            }
          })
        }
      },
      totalCount (val) {
        if (!val) {
          this.isShowCartList = false
        }
      }
    },
    computed: {
      totalCount () {
        let count = 0
        this.selectFoods.forEach((food) => {
          if (food.count > 0) {
            count += food.count
          }
        })
        return count
      },
      account () {
        let prices = 0
        this.selectFoods.forEach((food) => {
          if (food.count > 0) {
            prices += food.count * food.price
          }
        })
        return prices
      }
    }
  }
</script>

<style>
  @import "shopcart.css";
</style>
