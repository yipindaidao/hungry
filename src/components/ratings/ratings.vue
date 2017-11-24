<template>
    <div class="ratings">
        <div class="ratings-content">
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{ seller.score }}</h1>
                    <div class="desc">综合评分</div>
                    <div class="rank">高于周边商家{{ seller.rankRate }}%</div>
                </div>
                <div class="overview-right">
                    <div class="atitude">
                        <span class="title">服务态度</span>
                        <el-rate class="rate-hook" v-model="seller.serviceScore" disabled></el-rate>
                        <span class="score">{{ seller.serviceScore }}</span>
                    </div>
                    <div class="atitude">
                        <span class="title">商品评价</span>
                        <el-rate class="rate-hook" v-model="seller.foodScore" disabled></el-rate>
                        <span class="score">{{ seller.foodScore }}</span>
                    </div>
                    <div class="atitude delivery-hook">
                        <span class="title">送达时间</span>
                        <span class="deliveryTime">{{ seller.deliveryTime }}分钟</span>
                    </div>
                </div>
            </div>
            <split></split>
            <div class="selrating-wrapper">
                <selrating :ratingType="ratingType"  @select-type="selectType" @only-type="onlyType" :ratings="ratingArray"></selrating>
            </div>
            <ul class="ratings-wrapper">
                <li class="ratings-item" v-for="rating in ratingArray" v-show="ratingShow(rating.rateType,rating.text)">
                    <div class="item-left">
                        <img :src="rating.avatar" width="28" height="28"/>
                    </div>
                    <div class="item-right">
                        <div class="name">{{ rating.username }}</div>
                        <el-rate class="rate-hook" v-model="rating.score" disabled></el-rate>
                        <div class="content" v-show="rating.text">{{ rating.text }}</div>
                        <div class="recomend">
                            <span class="support-hook" :class="{'icon-thumb_up': rating.rateType === 0,'icon-thumb_down': rating.rateType === 1,'active': rating.rateType === 0}"></span>
                            <span class="recomend-item" v-show="rating.recommend" v-for="item in rating.recommend">{{ item }}</span>
                        </div>
                        <div class="time">{{ rating.rateTime | fmtDate }}</div>
                    </div>
                </li>
            </ul>

        </div>
    </div>
</template>

<script>
    import split from '../split/split'
    import selrating from '../selrating/selrating'
    import BScroll from 'better-scroll'
    import {formatDate} from '../../common/js/date'
    const ALL = 2
    const POSITIVE = 0
    const NEGTIVE = 1
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
           return {
               ratingArray: [],
               selRatingType: ALL,
               onlyContent: false,
               ratingType: {
                   all: '全部',
                   positive: '满意',
                   negtive: '不满意'
               }
           }
        },
        created() {
           this.$http.get('/api/ratings').then(res => {
               res = res.body
               this.ratingArray = res.data
               this.initBScroll()
           })
        },
        methods: {
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
            },
            initBScroll() {
                this.$nextTick(() => {
                    if(!this.fs) {
                        this.fs = new BScroll('.ratings',{
                            click: true
                        })
                    }
                    else {
                        this.fs.refresh()
                    }

                })
            }
        },
        filters: {
            fmtDate(time) {
                var date = new Date(time);
                return formatDate(date, 'yyyy-MM-dd hh:mm');
            }
        },
        components: {
            split,selrating
        }
    }
</script>

<style lang="scss">

    .ratings {
        position: absolute;
        left: 0;
        top: 174px;
        bottom: 0;
        width: 100%;
        overflow: hidden;
        .ratings-content {
            .overview {
                display: flex;
                padding: 18px 0;
                .overview-left {
                    flex: 0 0 130px;
                    width: 130px;
                    text-align: center;
                    border-right: 1px solid rgba(7,17,27,0.1);
                    .score {
                        font-size: 24px;
                        line-height: 28px;
                        color: rgb(255,153,0);
                    }
                    .desc {
                        font-size: 12px;
                        line-height: 12px;
                        color: rgb(7,17,27);
                        margin: 6px 0 8px 0;
                    }
                    .rank {
                        font-size: 10px;
                        line-height: 10px;
                        color: rgb(147,153,159);
                    }
                }
                .overview-right {
                    flex: 1;
                    padding-left: 24px;
                    .atitude {
                        &.delivery-hook {
                            margin-bottom: 0;
                        }
                        margin-bottom: 8px;
                        .title {
                            display: inline-block;
                            vertical-align: middle;
                            font-size: 12px;
                            line-height: 18px;
                            color: rgb(7,17,27);
                        }
                        .rate-hook {
                            display: inline-block;
                            vertical-align: middle;
                        }
                        .score,.deliveryTime {
                            display: inline-block;
                            vertical-align: middle;
                            font-size: 12px;
                            line-height: 18px;
                            color: rgb(255,153,0);
                        }
                        .deliveryTime {
                            color: rgb(147,153,159);
                        }
                    }
                }

            }
            .selrating-wrapper {
                margin-top: 18px;
            }
            .ratings-wrapper {
                padding: 0 18px ;
                .ratings-item {
                    display: flex;
                    padding: 18px 0;
                    border-bottom: 1px solid rgba(7,17,27,0.1);
                    .item-left {
                        flex: 0 0 18px;
                        margin-right: 12px;
                    }
                    .item-right {
                        flex: 1;
                        position: relative;
                        .name {
                            font-size: 10px;
                            line-height: 12px;
                            color: rgb(7,17,27);
                            margin-bottom: 4px;
                        }
                        .content {
                            margin-top: 6px;
                            margin-bottom: 8px;
                            font-size: 12px;
                            line-height: 18px;
                            color: rgb(7,17,27);
                        }
                        .recomend {
                            .support-hook {
                                font-size: 12px;
                                line-height: 16px;
                                color: rgb(147,153,159);
                                display: inline-block;
                                vertical-align: middle;
                                &.active {
                                    color: rgb(0,160,220);
                                }
                            }
                            .recomend-item {
                                display: inline-block;
                                vertical-align: middle;
                                font-size: 9px;
                                line-height: 16px;
                                color: rgb(147,153,159);
                                border: 1px solid rgba(7,17,7,0.1);
                                border-radius: 1px;
                                padding:2px 6px;
                                margin-right: 8px;
                                margin-bottom: 4px;
                            }
                        }
                        .time {
                            position: absolute;
                            top: 0px;
                            right: 0;
                            font-size: 10px;
                            line-height: 12px;
                            color: rgb(147,153,159);
                            font-weight: 200;
                        }
                    }
                }

            }
        }
    }

</style>
