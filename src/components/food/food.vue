<template>
    <transition name="show">
        <div class="food" v-show="showflag">
            <div class="food-content">
                <div class="image">
                    <img :src="selfood.image"/>
                    <i class="icon-arrow_lift" @click="hide"></i>
                </div>
                <div class="basic">
                    <h1 class="name">{{ selfood.name }}</h1>
                    <div class="sel-desc">
                        <span class="sell-count">月售{{ selfood.sellCount }}份</span>
                        <span class="rating">好评率{{ selfood.rating }}%</span>
                    </div>
                    <div class="price">
                        <span class="new">￥{{ selfood.price }}</span>
                        <span class="old" v-show="selfood.oldPrice">￥{{ selfood.oldPrice }}</span>
                    </div>
                    <transition name="fade">
                        <div class="join" v-show="!selfood.count || selfood.count===0" @click="joinTo">加入购物车</div>
                    </transition>
                    <div class="cartcontrol-wrapper">
                        <cartcontrol :selectfood="selfood" ref="cc"></cartcontrol>
                    </div>
                </div>
                <split></split>
                <div class="desc" v-show="selfood.description">
                    <h1 class="name">商品介绍</h1>
                    <div class="content">{{ selfood.description }}</div>
                </div>
                <split v-show="selfood.description"></split>
                <div class="rating">
                    <h1 class="title">商品评价</h1>
                    <selrating :ratingType="ratingType" :onlyContent="onlyContent" :selRatingType="selRatingType"></selrating>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    import cartcontrol from '../cartcontrol/cartcontrol'
    import split from '../split/split'
    import BScroll from 'better-scroll'
    import selrating from '../selrating/selrating'

    const ALL = 0
    const POSITIVE = 1
    const NEGTIVE = 2

    export default {
        props: {
            selfood: {}
        },
        data() {
            return {
                showflag: false,
                selRatingType: ALL,
                onlyContent: false,
                ratingType: {
                  all: '全部',
                  positive: '推荐',
                  negtive: '吐槽'
                }
            }
        },
        methods: {
            show() {
                this.showflag = true
                this.initBScroll()
            },
            hide() {
                this.showflag = false
            },
            joinTo(event) {
                this.$refs.cc.add(event)
            },
            initBScroll() {
                this.$nextTick(() => {
                    if(!this.fs) {
                        this.fs = new BScroll('.food',{
                            click: true
                        })
                    }
                    else {
                        this.fs.refresh()
                    }

                })
            }
        },
        components: {
            cartcontrol,split,selrating
        }
    }
</script>

<style type="text/css" lang="scss">
    .food {
        position: fixed;
        left: 0;
        top: 0;
        bottom: 46px;
        width: 100%;
        background: #fff;
        z-index: 30;
        transition:all 0.2s linear;
        &.show-enter,&.show-leave-to {
            transform: translate3d(100%,0,0);
        }
        .food-content {
            .image {
                position: relative;
                width: 100%;
                height: 0;
                padding-top: 100%;
                img {
                    position: absolute;
                    left: 0;
                    top: 0;
                    width: 100%;
                    height: 100%;
                }
                .icon-arrow_lift {
                    position: absolute;
                    left: 10px;
                    top: 10px;
                    width: 16px;
                    height: 16px;
                    color: #fff;
                    display: inline-block;
                }
            }
            .basic {
                position: relative;
                padding: 18px;
                .name {
                    font-size: 14px;
                    font-weight: 700;
                    color: rgb(7,17,27);
                    line-height: 14px;
                }
                .sel-desc {
                    margin-top: 8px;
                    .sell-count,.rating {
                        font-size: 10px;
                        line-height: 10px;
                        color: rgb(147,153,159);
                    }
                }
                .price {
                    margin-top: 18px;
                    .new {
                        font-size: 14px;
                        color: rgb(240,20,20);
                        line-height: 24px;
                        font-weight: 700;
                    }
                    .old {
                        font-size: 10px;
                        font-weight: 700;
                        line-height: 24px;
                        color: rgb(147,153,159);
                        text-decoration: line-through;
                    }
                }
                .join {
                    position: absolute;
                    right: 18px;
                    bottom: 18px;
                    font-size: 10px;
                    line-height: 12px;
                    padding: 6px 12px;
                    text-align: center;
                    background: rgb(0,160,220);
                    color: #fff;
                    border-radius: 24px;
                    z-index: 20;
                    opacity: 1;
                    transition:all 0.2s linear;
                    &.fade-enter,&.fade-leave-to {
                        opacity: 0;
                    }
                }
                .cartcontrol-wrapper {
                    position: absolute;
                    right: 18px;
                    bottom: 12px;
                }
            }
            .desc {
                padding: 18px;
                .name {
                    font-size: 14px;
                    color: rgb(7,17,27);
                    line-height: 14px;
                }
                .content {
                    margin-top: 6px;
                    padding: 0 8px;
                    font-size: 12px;
                    color: rgb(77,85,93);
                    line-height: 24px;
                    font-weight: 200;
                }
            }
            .rating {
                padding: 18px 18px 12px;
                .title {
                    font-size: 14px;
                    color: rgb(7,17,27);
                    line-height: 14px;
                    margin-bottom: 18px;
                }
            }
        }
    }

</style>
