<template>
  <div class="carcontrol">
    <div class="ico" @click="removeFromCart">
      <transition-group tag="div" name="move" class="transition-wrap">
        <div class="inner icon-remove_circle_outline"  v-show="food.count>0" :key="1"></div>
        <div class="cart-count" v-show="food.count>0" :key="2">{{food.count}}</div>
      </transition-group>
    </div>
    <div class="icon-add_circle ico" @click="addToCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue'

  export default {
    name: 'carcontrol',
    props: {
      food: {
        type: Object,
        default: {}
      }
    },
    data () {
      return {}
    },
    methods: {
      addToCart (event) {
        if (!event._constructed) {
          return false
        }
        event.stopPropagation()

        if (!this.food.count) {
          // 深度监听
          Vue.set(this.food, 'count', 1)
          this.food.count = 1
        } else {
          this.food.count += 1
        }
      },
      removeFromCart (event) {
        if (!event._constructed) {
          return false
        }
        event.stopPropagation()

        this.food.count -= 1
      }
    }
  }
</script>

<style>
  @import "./carcontrol.css";
</style>
