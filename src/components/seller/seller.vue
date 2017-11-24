<template>
    <div class="seller">
        <div class="seller-content">
            <div class="overview">
                <div class="overview-top">
                    <h1 class="title">{{ seller.name }}</h1>
                    <div class="rate-wrapper">
                        <el-rate class="rate-hook" v-model="seller.score" disabled></el-rate>
                        <span class="rating-count">({{ seller.ratingCount }})</span>
                        <span class="sell-count">月售{{ seller.sellCount }}单</span>
                    </div>
                    <div class="collect"></div>
                </div>
                <div class="overview-bottom">
                    <ul class="desc">
                        <li class="item">
                            <p class="title">起送价</p>
                            <span class="ct">{{ seller.minPrice }}<span class="dw">元</span></span>
                        </li>
                        <li class="item">
                            <p class="title">商家配送</p>
                            <span class="ct">{{ seller.deliveryPrice }}<span class="dw">元</span></span>
                        </li>
                        <li class="item">
                            <p class="title">平均配送时间</p>
                            <span class="ct">{{ seller.deliveryTime }}<span class="dw">分钟</span></span>
                        </li>
                    </ul>
                </div>
            </div>
            <split></split>
            <div class="notice">
                <h1 class="title">公告与活动</h1>
                <div class="bulletin">{{ seller.bulletin }}</div>
                <ul class="support">
                    <li class="support-item" v-for="item in seller.supports">
                        <span class="icon" :class="supportIcon[item.type]"></span>
                        <span class="text">{{ item.description }}</span>
                    </li>
                </ul>
            </div>
            <split></split>
            <div class="pics">
                <h1 class="title">商家实景</h1>
                <div class="pics-wrapper">
                    <ul class="pic-list" ref="picList">
                        <li class="pic-item" v-for="pic in seller.pics">
                            <img :src="pic" width="120" height="90"/>
                        </li>
                    </ul>
                </div>

            </div>
            <split></split>
            <div class="info">
                <h1 class="title">商家信息</h1>
                <ul class="info-list">
                    <li class="info-item" v-for="item in seller.infos">{{ item }}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    import split from '../split/split'
    import BScroll from 'better-scroll'
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                supportIcon: ["decrease", "discount", "special", "invoice", "guarantee"]
            }
        },
        mounted() {
            this.$nextTick(() => {
                this.initSellerScroll()
                this.initPicsScroll()
            })
        },
        methods: {
            initSellerScroll() {
                if(!this.fs) {
                    this.fs = new BScroll('.seller',{
                        click: true
                    })
                }
                else {
                    this.fs.refresh()
                }
            },
            initPicsScroll() {
                if(this.seller.pics) {
                    let picW = 120
                    let margin = 6
                    let totalW = (picW+margin)*this.seller.pics.length-margin
                    this.$refs.picList.style.width = totalW+'px'

                    if(!this.picScroll) {
                        this.picScroll = new BScroll('.pics-wrapper',{
                            scrollX: true,
                            eventPassthrough: 'vertical'
                        })
                    }
                    else {
                        this.picScroll.refresh()
                    }
                }

            }
        },
        components: {
            split
        }
    }
</script>

<style lang="scss">

    .seller {
        position: absolute;
        left: 0;
        top: 174px;
        bottom: 0;
        width: 100%;
        overflow: hidden;
        .seller-content {
            .overview {
                padding: 0 18px;
                .overview-top {
                    padding: 18px 0;
                    position: relative;
                    border-bottom: 1px solid rgba(7,17,27,0.1);
                    .title {
                        font-size: 14px;
                        line-height: 14px;
                        color: rgb(7,17,27);
                    }
                    .rate-wrapper {
                        margin-top: 8px;
                        font-size: 0;
                        .rate-hook {
                            display: inline-block;
                            vertical-align: middle;
                        }
                        .rating-count {
                            display: inline-block;
                            vertical-align: middle;
                            margin-left: 8px;
                            margin-right: 12px;
                            font-size: 10px;
                            line-height: 18px;
                            color: rgb(77,85,93);
                        }
                        .sell-count {
                            display: inline-block;
                            vertical-align: middle;
                            font-size: 10px;
                            line-height: 18px;
                            color: rgb(77,85,93);
                        }
                    }
                }
                .overview-bottom {
                    padding: 18px 0px;
                    .desc {
                        display: flex;
                        .item {
                            flex: 1;
                            text-align: center;
                            box-sizing: border-box;
                            &:nth-child(2) {
                                border-right: 1px solid rgba(7,17,27,0.1);
                                border-left: 1px solid rgba(7,17,27,0.1);
                            }
                            .title {
                                font-size: 10px;
                                line-height: 10px;
                                color: rgb(147,153,159);
                                margin-bottom: 4px;
                            }
                            .ct {
                                font-size: 24px;
                                line-height: 24px;
                                color: rgb(7,17,27);
                                .dw {
                                    font-size: 10px;
                                }
                            }
                        }
                    }
                }

            }
            .notice {
                padding: 18px 18px 0;
                .title {
                    font-size: 14px;
                    line-height: 14px;
                    color: rgb(7,17,27);
                }
                .bulletin {
                    margin-top: 8px;
                    font-size: 12px;
                    line-height: 24px;
                    color: rgb(240,20,20);
                    font-weight: 200;
                    padding: 0 12px 16px;
                    text-align: justify;
                }
                .support {
                    .support-item {
                        padding: 16px 0;
                        border-top: 1px solid rgba(7,17,27,0.1);
                        font-size: 0;
                        .icon {
                            display: inline-block;
                            margin-right: 6px;
                            width: 24px;
                            height: 24px;
                            background-size: cover;
                            background-repeat: no-repeat;
                            vertical-align: middle;
                            &.decrease {
                                background-image: url("./decrease_4@2x.png");
                            }
                            &.discount {
                                background-image: url("./discount_4@2x.png");
                            }
                            &.guarantee {
                                background-image: url("./guarantee_4@2x.png");
                            }
                            &.invoice {
                                background-image: url("./invoice_4@2x.png");
                            }
                            &.special {
                                background-image: url("./special_4@2x.png");
                            }
                        }
                        .text {
                            display: inline-block;
                            vertical-align: middle;
                            font-size: 12px;
                            line-height: 16px;
                            color: rgb(7,17,27);
                            font-weight: 200;
                        }
                    }
                }
            }
            .pics {
                padding: 18px;
                .title {
                    font-size: 14px;
                    line-height: 14px;
                    color: rgb(7,17,27);
                    margin-bottom: 12px;
                }
                .pics-wrapper {
                    width: 100%;
                    overflow: hidden;
                    white-space: nowrap;
                    .pic-list {
                        font-size: 0;
                        .pic-item {
                            display: inline-block;
                            margin-right: 6px;
                            box-sizing: border-box;
                            border: 1px solid rgba(7,17,27,0.1);
                        }
                    }
                }
            }
            .info {
                padding: 18px 18px 0;
                .title {
                    font-size: 14px;
                    line-height: 14px;
                    color: rgb(7,17,27);
                    margin-bottom: 12px;
                }
                .info-list {
                    .info-item {
                        padding: 16px 12px;
                        font-size: 12px;
                        line-height: 16px;
                        color: rgb(7,17,27);
                        font-weight: 200;
                    }
                }
            }
        }
    }

</style>
