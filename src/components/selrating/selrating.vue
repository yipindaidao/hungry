<template>
  <div class="selrating">
    <div class="rating-type">
      <span class="block positive">{{ ratingType.all }}<span class="count">54</span></span>
      <span class="block positive">{{ ratingType.positive }}<span class="count">27</span></span>
      <span class="block negtive">{{ ratingType.negtive }}<span class="count">10</span></span>
    </div>
  </div>
</template>

<script>
    export default {
        props: {
            ratingType: {},
            onlyContent: Boolean,
            selRatingType: Number
        },
        data () {
            return {
            }
        }
    }
</script>

<style type="text/css" lang="scss">
  .selrating {
    .rating-type {
      font-size: 0;
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
          }
        }
      }
    }
  }

</style>
