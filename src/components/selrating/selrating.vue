<template>
  <div class="selrating">
    <div class="rating-type">
      <span class="block positive" :class="{active: selfSelRatignType===2}" @click="choose(2)">{{ ratingType.all }}<span class="count">{{ ratings.length }}</span></span>
      <span class="block positive" :class="{active: selfSelRatignType===0}" @click="choose(0)">{{ ratingType.positive }}<span class="count">{{ positives.length }}</span></span>
      <span class="block negtive" :class="{active: selfSelRatignType===1}" @click="choose(1)">{{ ratingType.negtive }}<span class="count">{{ negtives.length }}</span></span>
    </div>
    <div class="switch" @click="only" :class="{on: selfOnlyContent}">
      <i class="icon-check_circle" ></i><span>只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
    const ALL = 2
    const POSITIVE = 0
    const NEGTIVE = 1

    export default {
        props: {
            ratingType: {},
            ratings: {
                type: Array,
                default() {
                    return []
                }
            }
        },
        data () {
            return {
                selfSelRatignType: ALL,
                selfOnlyContent: false
            }
        },
        computed: {
           positives() {
               return this.ratings.filter(rating => {
                   return rating.rateType === POSITIVE
               })
           },
            negtives() {
                return this.ratings.filter(rating => {
                    return rating.rateType === NEGTIVE
                })
            }
        },
        methods: {
            choose(tp) {
                this.selfSelRatignType = tp
                this.$emit('select-type',this.selfSelRatignType)
            },
            only() {
                this.selfOnlyContent = !this.selfOnlyContent
                this.$emit('only-type',this.selfOnlyContent)
            }
        }
    }
</script>

<style type="text/css" lang="scss">
  .selrating {

    .rating-type {
      font-size: 0;
      margin: 0 18px;
      padding-bottom: 12px;
      border-bottom: 1px solid rgba(7,17,27,0.1);
      .block {
        display: inline-block;
        padding: 8px 12px;
        font-size: 12px;
        color: rgb(77,85,93);
        line-height: 16px;
        margin-right: 8px;
        border-radius: 2px;
        .count {
          font-size: 8px;
          margin-left: 2px;
        }
        &.positive {
          background: rgba(0,160,220,0.2);
          &.active {
            color: #fff;
            background: rgb(0,160,220);
          }
        }
        &.negtive {
          background: rgba(77,85,93,0.2);
          &.active {
            background: rgb(77,85,93);
            color: #fff;
          }
        }
      }
    }
    .switch {
      padding: 12px 18px;
      font-size: 0;
      border-bottom: 1px solid rgba(7,17,27,0.2);
      &.on {
        i {
          color: #00c850;
        }
      }
      i {
        display: inline-block;
        font-size: 24px;
        line-height: 24px;
        color: rgb(147,153,159);
        vertical-align: middle;
        margin-right: 4px;
      }
      span {
        display: inline-block;
        font-size: 12px;
        line-height: 24px;
        color: rgb(147,153,159);
        vertical-align: middle;
      }
    }
  }

</style>
