<template>
  <div class="rating-group">
    <div class="like-tab border-1px">
      <div :class="allShowClass" @click="showRating('all', $event)">
        {{typeContent.all}}&nbsp;<span class="num">{{ratings.length}}</span>
      </div>
      <div :class="likeShowClass" @click="showRating('like', $event)">
        {{typeContent.like}}&nbsp;<span class="num">{{likeCount}}</span>
      </div>
      <div :class="unLikeShowClass" @click="showRating('unlike', $event)">
        {{typeContent.unlike}}&nbsp;<span class="num">{{unlikeCount}}</span>
      </div>
    </div>

    <div class="with-content border-1px" @click="onlyWithContent($event)">
      <i :class="['icon-check_circle', 'icon', {active: isWithContent}]"></i>
      <p class="des">只看有内容的评价</p>
    </div>

    <div class="rating-list" ref="rateWrap">
      <ul class="rating-ul" v-if="isNormalType">
        <li class="rating-li border-1px" v-for="rating in showRatings"  v-if="filterNullText(rating)">

          <div class="left-wrap">
            <img :src="rating.avatar" alt="" width='28' height="28" class="avatar">
          </div>

          <div class="right-wrap">

            <div class="from-time">
              <div class="from">
                {{rating.username}}
              </div>
              <div class="time">
                {{translateFormatDate(rating.rateTime)}}
              </div>
            </div>

            <div class="star-time">
              <div class="star-rate">
                <star-group :score="rating.score"></star-group>
              </div>
              <div class="delivery-time" v-show="rating.deliveryTime > 0">
                {{rating.deliveryTime}}分钟送达
              </div>
            </div>

            <div class="rating-content">
              {{rating.text}}
            </div>

            <div class="des">
              <div :class="upDownClass(!rating.rateType)"></div>
              <ul class="recommends">
                <li class="recommend ellipsis" v-for="reco in rating.recommend">
                  {{reco}}
                </li>
              </ul>
            </div>

          </div>

        </li>
      </ul>
      <ul class="normal-rating-ul" v-if="!isNormalType">
        <li class="rating-li border-1px" v-for="rating in showRatings" v-if="filterNullText(rating)">

          <div class="top-line">
            <div class="time">
              {{translateFormatDate(rating.rateTime)}}
            </div>

            <div class="from-img">
              <div class="from">
                {{rating.username}}
              </div>
              <img :src="rating.avatar" alt="" width='12' height="12" class="avatar">
            </div>
          </div>

          <div class="bottom-line">
            <div :class="upDownClass(!rating.rateType)"></div>
            <p class="rating-content">
              {{rating.text}}
            </p>
          </div>

        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import starGroup from '../star-group/star-group'

  const typeContent = {
    all: '全部',
    like: '满意',
    unlike: '不满意'
  }

  export default {
    name: 'rating-group',
    components: {
      'star-group': starGroup
    },
    props: {
      ratings: {
        type: Array,
        default () {
          return []
        }
      },
      simple: {
        type: Boolean,
        default: false,
        required: false
      },
      typeContent: {
        type: Object,
        default () {
          return typeContent
        },
        required: false
      }
    },
    data () {
      return {
        rateScroll: {},
        isWithContent: false,
        showRatings: this.ratings,
        isShowAll: true,
        isShowLike: false,
        isShowUnlike: false
      }
    },
    computed: {
      isNormalType () {
        return this.simple === false
      },
      allShowClass () {
        return [
          `all`,
          `tab-item`,
          {
            'active': this.isShowAll
          }]
      },
      likeShowClass () {
        return [
          `like`,
          `tab-item`,
          {
            'active': this.isShowLike
          }]
      },
      unLikeShowClass () {
        return [
          `unlike`,
          `tab-item`,
          {
            'active': this.isShowUnlike
          }]
      },
      unlikeCount () {
        let count = 0
        this.ratings.forEach(function (rating) {
          if (rating.rateType) {
            count += 1
          }
        })
        return count
      },
      likeCount () {
        let count = 0
        this.ratings.forEach(function (rating) {
          if (!rating.rateType) {
            count += 1
          }
        })
        return count
      }
    },
    methods: {
      filterRatings (type) {
        let resultRating = []
        this.isShowAll = false
        this.isShowLike = false
        this.isShowUnlike = false

        switch (type) {
          case 'all':
            this.isShowAll = true
            resultRating = this.ratings.filter(function (rating) {
              return rating
            })
            break

          case 'like':
            this.isShowLike = true
            resultRating = this.ratings.filter(function (rating) {
              return !rating.rateType ? rating : false
            })
            break

          case 'unlike':
            this.isShowUnlike = true
            resultRating = this.ratings.filter(function (rating) {
              return rating.rateType ? rating : false
            })
            break

          // no default
        }
//        console.log(resultRating)
        return resultRating
      },
      showRating (type, event) {
        // 防止出现多次点击
        if (!event._constructed) {
          return false
        }

        this.showRatings = this.filterRatings(type)
      },
      filterNullText (rating) {
//        console.log(!(rating.text === '' && this.isWithContent))
        return !(rating.text === '' && this.isWithContent)// 若选中“仅显示有内容的评价”，且当前项text为空
      },
      onlyWithContent (event) {
        // 防止出现多次点击
        if (!event._constructed) {
          return false
        }
        this.isWithContent = !this.isWithContent
//        console.log(this.isWithContent)
      },
      upDownClass (isUp) {
        return [
          `up-down`,
          {
            'icon-thumb_up': isUp,
            'icon-thumb_down': !isUp
          }
        ]
      },
      initScroll () {
        this.rateScroll = new BScroll(this.$refs.rateWrap, {
          probeType: 3,
          click: true
        })
      },
      /**
       * 转化日期格式为YYYY-MM-DD HH:MM:SS
       * @param time
       */
      translateFormatDate (time) {
        let date = new Date(time)

        let YMD = date.getFullYear() + '-' + this.addZero(date.getMonth() + 1) + '-' + this.addZero(date.getDate())

        let HM = this.addZero(date.getHours()) + ':' + this.addZero(date.getMinutes())

        return YMD + ' ' + HM
      },
      /**
       * 给num补位0,
       * @param num
       */
      addZero (num) {
        return num >= 10 ? num : '0' + num
      }
    },
    mounted () {
      this.initScroll()
    }
  }
</script>

<style>
  @import "./rating-group.css";
</style>
