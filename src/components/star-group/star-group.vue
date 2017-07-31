<template>
  <div class="star-group">
    <div class="stars">
      <div v-for='(item, index) in starCount'
           :class="starClasses(index)">
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const prefixCls = 'star'

  export default {
    name: 'star-group',
    props: {
      starCount: {
        type: Number,
        default: 5
      },
      score: {
//        分数，5分制
        type: Number,
        default: 0
      }
    },
    methods: {
      starClasses (index) {
        index += 1

        return [
          `${prefixCls}`,
          {
            [`${prefixCls}-on`]: index <= this.score, // 选中前一位
            [`${prefixCls}-off`]: index > this.score && index !== this.ceilStar,
            [`${prefixCls}-half`]: index > this.score && this.hasHalfStar && index === this.ceilStar
          }
        ]
      }
    },
    computed: {
      hasHalfStar () {
        return this.score % 1
      },
      ceilStar () {
        // 获取选中星个数
        return Math.ceil(this.score)
      }
    }
  }
</script>

<style>
  @import "star-group.css";
</style>
