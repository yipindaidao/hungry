<template>
  <div class="cartcontrol">
    <transition name="roll">
      <div class="cart-decrease" v-show="selectfood.count>0" @click.stop.prevent="decrease">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="selectfood.count>0">{{ selectfood.count }}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="add"></div>
  </div>
</template>

<script>
    import Vue from 'vue'
    export default {
        props: {
            selectfood: {
                type: Object
            }
        },
        data () {
            return {
            }
        },
        methods: {
            add(event) {
                if(!this.selectfood.count) {
                    Vue.set(this.selectfood,'count',1)
                }
                else {
                    this.selectfood.count++
                }
                //向父组件传递事件
                this.$emit('cartAddEvent',event.target)
            },
            decrease() {
                this.selectfood.count--
            }
        }
    }
</script>

<style type="text/css" lang="scss">
  .cartcontrol {
    font-size: 0;
    .cart-decrease {
      display: inline-block;
      padding: 6px;
      transition: all 0.4s linear;
      .inner {
        display: inline-block;
        font-size: 24px;
        line-height: 24px;
        color: rgb(0,160,220);
        transition: all 0.4s linear;
      }
      &.roll-enter,&.roll-leave-to {
        opacity: 0;
        transform: translate3d(24px,0,0);
        .inner {
          transform: rotate(180deg);
        }
      }
    }
    .cart-count {
      display: inline-block;
      font-size: 10px;
      line-height: 24px;
      color: rgb(147,153,159);
      vertical-align: top;
      width: 12px;
      text-align: center;
      padding-top: 6px;
    }
    .cart-add {
      display: inline-block;
      font-size: 24px;
      line-height: 24px;
      color: rgb(0,160,220);
      padding: 6px;
    }
  }
</style>
