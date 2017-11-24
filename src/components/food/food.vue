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
                    <selrating :ratingType="ratingType"  @select-type="selectType" @only-type="onlyType" :ratings="selfood.ratings"></selrating>
                    <div class="rating-wrapper">
                        <ul v-show="selfood.ratings && selfood.ratings.length">
                            <li v-for="rating in selfood.ratings" class="rating-item" v-show="ratingShow(rating.rateType,rating.text)">
                                <div class="time">{{ rating.rateTime | fmtDate }}</div>
                                <div class="content">
                                    <span class="support-hook" :class="{'icon-thumb_up': rating.rateType === 0,'icon-thumb_down': rating.rateType === 1,'active': rating.rateType === 0}"></span>
                                    <span class="name">{{ rating.text }}</span>
                                </div>
                                <div class="user">
                                    <span class="name">{{ rating.username }}</span>
                                    <img :src="rating.avatar" width="12" height="12"/>
                                </div>
                            </li>
                        </ul>
                        <div class="no-rating" v-show="!selfood.ratings || !selfood.ratings.length">暂无评价</div>
                    </div>
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
    import {formatDate} from '../../common/js/date'

    const ALL = 2
    const POSITIVE = 0
    const NEGTIVE = 1

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
            },
            selectType(tp) {
                this.selRatingType = tp
                this.$nextTick(() => {
                    this.fs.refresh()
                })
            },
            onlyType(ot) {
                this.onlyContent = ot
                this.$nextTick(() => {
                    this.fs.refresh()
                })
            },
            ratingShow(rateType,text) {
                if(this.onlyContent && !text) {
                    return false
                }
                if(this.selRatingType === ALL) {
                    return true
                }
                else {
                    return rateType === this.selRatingType
                }
            }
        },
        filters: {
            fmtDate(time) {
                var date = new Date(time);
                return formatDate(date, 'yyyy-MM-dd hh:mm');
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
                    padding: 5px;
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
                .title {
                    padding: 18px 18px 0;
                    font-size: 14px;
                    color: rgb(7,17,27);
                    line-height: 14px;
                    margin-bottom: 18px;
                }
                .rating-wrapper {
                    padding: 0 18px;
                    .rating-item {
                        padding: 16px 0;
                        position: relative;
                        border-bottom: 1px solid rgba(7,17,27,0.1);
                        .time {
                            font-size: 10px;
                            line-height: 12px;
                            color: rgb(147,153,159);
                            margin-bottom: 6px;
                        }
                        .content {
                            .support-hook {
                                font-size: 12px;
                                line-height: 24px;
                                color: rgb(147,153,159);
                                display: inline-block;
                                vertical-align: middle;
                                &.active {
                                    color: rgb(0,160,220);
                                }
                            }
                            .name {
                                display: inline-block;
                                vertical-align: middle;
                                font-size: 12px;
                                line-height: 16px;
                                color: rgb(7,17,27);
                            }
                        }
                        .user {
                            position: absolute;
                            right: 0;
                            top: 12px;
                            .name {
                                display: inline-block;
                                vertical-align: middle;
                                font-size: 10px;
                                line-height: 12px;
                                color: rgb(147,153,159);
                            }
                            img {
                                vertical-align: middle;
                            }
                        }
                    }
                    .no-rating {
                        padding: 16px 0;
                        font-size: 12px;
                        color: rgb(147,153,159);
                    }
                }
            }
        }
    }

</style>
