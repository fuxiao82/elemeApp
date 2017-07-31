<template>
  <div class="ratings">

    <div class="top-wrap border-1px">
      <div class="total-rate">
        <div class="score">
          3.9
        </div>
        <div class="title">
          综合评分
        </div>
        <div class="compare">
          高于周边商家69.2%
        </div>
      </div>

      <div class="total-star">

        <div class="service-star star-box">
          <div class="title">服务态度</div>
          <div class="star-rate">
            <star-group :score="3.9"></star-group>
          </div>
          <div class="score">3.9</div>
        </div>

        <div class="send-star star-box">
          <div class="title">配送态度</div>
          <div class="star-rate">
            <star-group :score="4"></star-group>
          </div>
          <div class="score">4.0</div>
        </div>

        <div class="deliver-time star-box">
          <div class="title">送达时间</div>
          <div class="minute">44分钟</div>
        </div>

      </div>
    </div>


    <rating-group :ratings="ratings" ref="ratingGroup" v-if="showRatingGroup"></rating-group>

  </div>
</template>

<script type="text/ecmascript-6">
  import ratingGroup from '../rating-group/rating-group.vue'
  import starGroup from '../star-group/star-group'

  export default {
    name: 'ratings',
    components: {
      'rating-group': ratingGroup,
      'star-group': starGroup
    },
    data () {
      return {
        ratings: [],
        showRatingGroup: false
      }
    },
    created () {
      this.$http.get('/api/ratings').then(response => {
        this.ratings = response.body.data

        this.$nextTick(() => {
          this.showRatingGroup = true
        })
      })
    }
  }
</script>

<style>
  @import 'ratings.css';
</style>
