<template>
    <div class="goods">
      <div class="menu-wrapper">
        <ul class="menu">
          <li class="menu-item" v-for="item in goods">
            <span class="text">{{ item.name }}</span>
          </li>
        </ul>
      </div>
      <div class="goods-wrapper">
        <ul>
          <li v-for="item in goods" class="good-item">
            <h1 class="title">{{ item.name }}</h1>
            <ul class="foods">
              <li v-for="food in item.foods" class="food-item">
                <div class="icon">
                  <img width="57" height="57" :src="food.icon"/>
                </div>
                <div class="content">
                  <h2 class="name">{{ food.name }}</h2>
                  <p class="desc">{{ food.description }}</p>
                  <p><span>月售{{ food.sellCount }}份</span><span>好评率{{ food.rating }}%</span></p>
                  <p><span>￥{{ food.price }}</span><span v-if="food.oldPrice">￥{{ food.oldPrice }}</span></p>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
  import BScroll from 'better-scroll'
    export default {
      data() {
        return {
          goods: []
        }
      },
      created() {
        this.$http.get('/api/goods').then(res => {
          res = res.body
          this.goods = res.data
          this.initBScroll()
        })
      },
      methods: {
        initBScroll() {
          this.$nextTick(() => {
            this.menuscroll = new BScroll('.menu-wrapper')
          })
        }
      }
    }
</script>

<style lang="scss">

  .goods {
    display: flex;
    position: absolute;
    left: 0;
    top: 174px;
    bottom: 46px;
    overflow: hidden;
    .menu-wrapper {
      background: #f3f5f7;
      width: 80px;
      .menu {
        .menu-item {
          box-sizing: border-box;
          width: 60px;
          height: 54px;
          line-height: 14px;
          display: table;
          margin: 0 auto;
          .text {
            display: table-cell;
            vertical-align: middle;
            font-size: 12px;
            color: rgb(77,85,93);
            border-bottom: 1px solid rgba(7,17,27,0.1);
          }
        }
      }
    }
    .goods-wrapper {
      flex: 1;
      .good-item {
        .title {
          height: 26px;
          line-height: 26px;
          background: #f3f5f7;
          border-left: 2px solid #d9dde1;
          font-size: 12px;
          color: rgb(147,153,159);
          padding-left: 14px;
        }
        .foods {
          .food-item {
            margin: 0 18px;
            padding: 18px 0;
            display: flex;
            .icon {
              width: 57px;
            }
            .content {
              flex: 1;
            }
          }
        }
      }
    }
  }

</style>
