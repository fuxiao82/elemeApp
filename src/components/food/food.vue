<template>
  <div class="food-detail" ref="foodDetailWrap">

    <div class="food-detail-wrap">
      <div class="img-wrap border-1px">
        <img :src="selectedFood.icon" alt="" height="375"/>
      </div>

      <div class="food-title-info">
        <p class="title">{{selectedFood.name}}</p>

        <div class="sell-info">
          <p class="sell-count">月售{{selectedFood.sellCount}}份</p>
          <p class="rating-percent">好评率{{selectedFood.rating}}%</p>
        </div>

        <div class="price-card">
          <price-group :price="selectedFood.price" :old-price="selectedFood.oldPrice"></price-group>
          <div class="add-to-cart">
            <button class="add-btn" @click="addToCart" v-if="!selectedFood.count">加入购物车</button>
            <carcontrol :food="selectedFood" v-if="selectedFood.count"></carcontrol>
          </div>
        </div>
      </div>

      <split-bar></split-bar>

      <div class="food-des" v-if="selectedFood.info">
        <h3 class="title">商品介绍</h3>
        <p class="description">{{selectedFood.info}}</p>
      </div>

      <split-bar v-if="selectedFood.info"></split-bar>

      <div class="food-rating">
        <h3 class="title">商品评价</h3>
        <rating-group :ratings="selectedFood.ratings" :simple="true" :type-content="typeContent"></rating-group>
      </div>

    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'
  import BScroll from 'better-scroll'
  import ratingGroup from '../rating-group/rating-group'
  import priceGroup from '../price-group/price-group'
  import splitBar from '../split-bar/split-bar'
  import carcontrol from '../carcontrol/carcontrol'

  const typeContent = {
    all: '全部',
    like: '推荐',
    unlike: '吐槽'
  }

  export default {
    name: 'food',
    components: {
      'rating-group': ratingGroup,
      'price-group': priceGroup,
      'split-bar': splitBar,
      carcontrol
    },
    props: {
      selectedFood: {
        type: Object,
        default: {}
      }
    },
    data () {
      return {
        typeContent: typeContent
      }
    },
    methods: {
      addToCart () {
        Vue.set(this.selectedFood, 'count', 1)
        this.selectedFood.count += 1

        this.$emit('addToCart', {
          food: this.selectedFood,
          count: this.selectedFood.count
        })
      }
    },
    mounted () {
      console.log(this.selectedFood)
      this.foodDetailScroll = new BScroll(this.$refs.foodDetailWrap, {
        probeType: 3,
        click: true
      })
    }
  }
</script>

<style>
  @import "./food.css";
</style>
