<template>
  <div class="shopcart">
    <div class="shopcart-left">
      <div class="logo-wrapper">
        <div class="logo"><i class="icon-shopping_cart"></i></div>
        <div class="badge">
          <el-badge :value="totalCount" class="item">
          </el-badge>
        </div>
      </div>
      <div class="price">￥{{ totalPrice }}</div>
      <div class="desc">另需配送费￥{{ deliveryPrice }}</div>
    </div>
    <div class="shopcart-right">￥{{ minPrice }}元起送</div>
  </div>
</template>

<script>
    export default {
        props: {
            selectFoods: {
                type: Array,
                default() {
                    return []
                }
            },
            deliveryPrice: {
                type: Number,
                default: 0
            },
            minPrice: {
                type: Number,
                default: 0
            }
        },
        data () {
            return {
            }
        },
        computed: {
            totalPrice() {
                let price = 0
                this.selectFoods.forEach(food => {
                    price += food.price * food.count
                })
                return price
            },
            totalCount() {
                let count = 0
                this.selectFoods.forEach(food => {
                    count += food.count
                })
                return count
            }
        }
    }
</script>

<style type="text/css" lang="scss">
  .shopcart {
    position: fixed;
    left: 0;
    bottom: 0;
    height: 46px;
    width: 100%;
    z-index: 50;
    background: #141d27;
    display: flex;
    .shopcart-left {
      flex: 1;
      height: 100%;
      font-size: 0px;
      .logo-wrapper {
        position: relative;
        top: -12px;
        margin: 0 12px;
        width: 56px;
        height: 56px;
        padding: 6px;
        box-sizing: border-box;
        border-radius: 50%;
        background: #141d27;
        display: inline-block;
        vertical-align: top;
        .logo {
          width: 100%;
          height: 100%;
          text-align: center;
          border-radius: 50%;
          box-sizing: border-box;
          background: #2b343c;
          .icon-shopping_cart {
            color: rgba(255,255,255,0.4);
            line-height: 44px;
            font-size: 24px;
          }
        }
        .badge {
          position: absolute;
          right: 0;
          top: 0;
        }
      }
      .price {
        display: inline-block;
        font-size: 16px;
        font-weight: 700;
        color: rgba(255,255,255,0.4);
        margin-top: 15px;
        margin-right: 12px;
        padding-right: 12px;
        border-right: 1px solid rgba(255,255,255,0.1);
      }
      .desc {
        display: inline-block;
        font-size: 16px;
        font-weight: 200;
        color: rgba(255,255,255,0.4);
      }
    }
    .shopcart-right {
      width: 105px;
      height: 100%;
      font-size: 12px;
      color: rgba(255,255,255,0.4);
      text-align: center;
      line-height: 46px;
      background: #2b343c;
    }
  }
</style>
