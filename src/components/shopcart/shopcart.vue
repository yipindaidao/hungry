<template>
  <div class="shopcart">
    <div class="shopcart-left">
      <div class="logo-wrapper">
        <div class="logo" :class="{highlight: totalCount > 0}"><i class="icon-shopping_cart" :class="{highlight: totalCount > 0}"></i></div>
        <div class="badge">
          <el-badge :value="totalCount" class="item">
          </el-badge>
        </div>
      </div>
      <div class="price" :class="{highlight: totalPrice > 0}">￥{{ totalPrice }}</div>
      <div class="desc">另需配送费￥{{ deliveryPrice }}</div>
    </div>
    <div class="shopcart-right" :class="{enough: this.totalPrice >= this.minPrice}">{{ payDesc }}</div>
    <transition-group name="drop"
                      v-on:before-enter="beforeEnter"
                      v-on:enter="enter"
                      v-on:after-enter="afterEnter">
      <div class="ball" v-for="ball in balls" v-show="ball.show" :key="ball.id">
        <div class="inner inner-hook"></div>
      </div>
    </transition-group>

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
                balls: [
                    {id: 1,show: false},
                    {id: 2,show: false},
                    {id: 3,show: false},
                    {id: 4,show: false},
                    {id: 5,show: false}
                ],
                dropedballs: []
            }
        },
        methods: {
            drop(target) {//el:+
                for(let i=0;i<this.balls.length;i++) {
                    let ball = this.balls[i]
                    if(!ball.show) {
                        ball.show = true
                        ball.el = target
                        this.dropedballs.push(ball)
                        return
                    }
                }
            },
            beforeEnter(el) {
                for(let i=0;i<this.dropedballs.length;i++) {
                    let ball = this.dropedballs[i]
                    //获取+相对于屏幕的位置
                    let rect = ball.el.getBoundingClientRect()
                    let x = rect.left - 32
                    let y = -rect.top
                    el.style.display = ''
                    el.style.webkitTransform = `translate3d(0,${y}px,0)`
                    el.style.transform = `translate3d(0,${y}px,0)`
                    let inner = el.getElementsByClassName('inner-hook')[0]
                    inner.style.webkitTransform = `translate3d(${x}px,0,0)`
                    inner.style.transform = `translate3d(${x}px,0,0)`
                }
            },
            enter(el, done) {
                let rf = el.offsetHeight
                this.$nextTick(() => {
                    el.style.webkitTransform = 'translate3d(0,0,0)'
                    el.style.transform = 'translate3d(0,0,0)'
                    let inner = el.getElementsByClassName('inner-hook')[0]
                    inner.style.webkitTransform = 'translate3d(0,0,0)'
                    inner.style.transform = 'translate3d(0,0,0)'
                })
            },
            afterEnter(el) {
                let ball = this.dropedballs.shift()
                if(ball) {
                    ball.show = false
                }
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
            },
            payDesc() {
                if(this.totalPrice === 0) {
                    return `￥${this.minPrice}元起送`
                }
                else  if(this.totalPrice < this.minPrice) {
                    return `还差￥${this.minPrice - this.totalPrice}元起送`
                }
                else {
                    return '去结算'
                }
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
          &.highlight {
            background: rgb(0,160,220);
          }
          .icon-shopping_cart {
            color: rgba(255,255,255,0.4);
            line-height: 44px;
            font-size: 24px;
            &.highlight {
              color: #fff;
            }
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
        &.highlight {
          color: #fff;
        }
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
      font-weight: 700;
      &.enough {
        background: #00b43c;
        color: #fff;
      }
    }
    .ball {
      position: fixed;
      left: 32px;
      bottom: 22px;
      z-index: 100;
      transition: all 0.4s;
      .inner {
        width: 16px;
        height: 16px;
        border-radius: 50%;
        background: rgb(0,160,220);
        transition: all 0.4s;
      }
    }
  }
</style>
