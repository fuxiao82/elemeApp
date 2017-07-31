<template>
  <div :class="goodWrapClass" >

    <template v-if="!selectedFood">
      <div class="menu-wrap" ref="menuWrap">
        <div class="menu-container">
          <div v-for="(menu, i) in goods"
               :class="['menu-box',{'active':i === currentIndex}]"
               @click="selectMenu(i, $event)">
            <div class="menu-item border-1px">
              <p class="text">
                <span :class="['icon', supportType[menu.type]]"></span>
                {{menu.name}}
              </p>
            </div>
          </div>
        </div>
      </div>
      <div class="foods-wrap" ref="foodWrap">
        <ul class="foods-container">
          <li class="foods-box" v-for="(item, j) in goods">
            <div class="menu-name">
              <div class="food-icon"></div>
              {{item.name}}
            </div>
            <div class="food-item border-1px" v-for="(food, k) in item.foods"  @click="viewFood(food, $event)">
              <img :src="food.image" alt="" width="57" height="57" class="food-icon">

              <div class="food-detail">

                <div class="food-name">
                  {{food.name}}
                </div>

                <div class="food-des">
                  <p>{{food.description}}</p>
                </div>

                <div class="sell-rating">
                  <div class="month-sell">
                    月售{{food.ratings.length}}份
                  </div>

                  <div class="food-rating">
                    好评率{{food.rating}}%
                  </div>
                </div>

                <div class="price-card">
                  <price-group :price="food.price" :old-price="food.oldPrice"></price-group>
                  <carcontrol :food="food"></carcontrol>
                </div>

              </div>

            </div>
          </li>
        </ul>
      </div>
    </template>

    <transition name="slide-right">
      <food :selected-food="selectedFood" v-if="selectedFood" @addToCart="addToCart" :select-count="selectedFood.count"></food>
    </transition>

    <shopcart :deliver-price="seller.deliveryPrice"
              :min-price="seller.minPrice"
              :select-foods="checkFoods">
    </shopcart>

  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import shopcart from '../shopcart/shopcart.vue'
  import carcontrol from '../carcontrol/carcontrol.vue'
  import food from '../food/food.vue'
  import priceGroup from '../price-group/price-group'

  export default {
    components: {
      shopcart,
      carcontrol,
      food,
      'price-group': priceGroup
    },
    name: 'goods',
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: [],
        menuScroll: {},
        foodScroll: {},
        scrollY: '',
        listHeight: [],
        supportType: ['decrease', 'discount', 'guarantee', 'invoice', 'special'],
        selectedFood: null
      }
    },
    methods: {
      addToCart () {
        console.log('通知父组件成功')
//        this.checkFoods
      },
      viewFood (food, event) {
        // 防止出现多次点击
        if (!event._constructed) {
          return false
        }

        this.selectedFood = food
      },
      selectMenu (index, event) {
        // 防止出现多次点击
        if (!event._constructed) {
          return false
        }

        var targetEl = this.$refs.foodWrap.getElementsByClassName('foods-box')[index]
        this.foodScroll.scrollToElement(targetEl, 300)
      },
      initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrap, {
          click: true
        })
        this.foodScroll = new BScroll(this.$refs.foodWrap, {
          probeType: 3,
          click: true
        })

        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      caculateListHeight () {
        let foodList = this.$refs.foodWrap.getElementsByClassName('foods-box')
        let height = 0
        this.listHeight.push(height)

        for (let i = 0; i < foodList.length; i++) {
          height += foodList[i].clientHeight
          this.listHeight.push(height)
        }
      }
    },
    computed: {
      goodWrapClass () {
        return [
          `goods-wrap`,
          {
            'active-food': this.selectedFood
          }]
      },
      currentIndex () {
        let listHeight = this.listHeight
        for (let i = 0; i < listHeight.length; i++) {
          var heightA = listHeight[i]
          var heightB = listHeight[i + 1]

          if (!heightB || heightA <= this.scrollY && this.scrollY < heightB) {
            return i
          }
        }
        return 0
      },
      checkFoods () {
        let select = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count > 0) {
              select.push(food)
            }
          })
        })
        return select
      }
    },
    created () {
      this.$http.get('/api/goods').then(response => {
        this.goods = response.body.data
        this.$nextTick(() => {
          this.initScroll()
          this.caculateListHeight()
        })
      })
    }
  }
</script>

<style>
  @import "goods.css";
</style>
