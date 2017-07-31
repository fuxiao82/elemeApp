<template>
    <div class="price-group">
        <div class="food-price-now">
          <span class="price-icon">￥</span>{{price}}
        </div>

        <div class="food-old-price" v-if="!(oldPrice == '')">
          <span class="price-icon">￥</span>{{oldPrice}}
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
  export default {
    name: 'price-group',
    props: {
      price: {
        type: Number,
        default: 0
      },
      oldPrice: {
        type: [Number, String],
        default: ''
      }
    }
  }
</script>

<style>
    @import "./price-group.css";
</style>
