<template>
    <div class="header">
        <div class="content-wrapper">
            <div class="avatar"><img :src="seller.avatar" width="64" height="64"/></div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{ seller.name }}</span>
                </div>
                <div class="desc">{{ seller.description }}/{{ seller.deliveryTime }}分钟送达</div>
                <div v-if="seller.supports" class="supports">
                    <span class="icon" :class="supportIcon[seller.supports[0].type]"></span>
                    <span class="text">{{ seller.supports[0].description }}</span>
                </div>
            </div>
            <div v-if="seller.supports" class="support-count" @click="showDetail">
                <span class="count">{{ seller.supports.length }}个</span>
                <span class="arrowr icon-keyboard_arrow_right"></span>
            </div>
        </div>
        <div class="bulletin-wrapper">
            <span class="icon"></span>
            <span class="text">{{ seller.bulletin }}</span>
            <span class="icon-keyboard_arrow_right"></span>
        </div>
        <div class="background">
            <img :src="seller.avatar" width="100%" height="100%"/>
        </div>
        <transition name="fade">
            <div v-show="detailShow" class="detail">
                <div class="detail-wrapper">
                    <div class="title-wrapper">
                        <div class="title">粥品香坊（大运村）</div>
                        <el-rate
                                v-model="3.5"
                                disabled
                        >
                        </el-rate>
                    </div>
                    <div class="separate">
                        <div class="line"></div>
                        <div class="title">优惠信息</div>
                        <div class="line"></div>
                    </div>
                </div>
                <div class="detail-close" @click="detailClose">
                    <i class="icon-close"></i>
                </div>
            </div>
        </transition>

    </div>
</template>

<script>
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                supportIcon: ["decrease", "discount", "special", "invoice", "guarantee"],
                detailShow: false
            }
        },
        methods: {
            detailClose() {
                this.detailShow = false
            },
            showDetail() {
                this.detailShow = true
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
    .header {
        position: relative;
        overflow: hidden;
        .content-wrapper {
            box-sizing: border-box;
            position: relative;
            padding: 24px 12px 18px 24px;
            background-color: rgba(7, 17, 27, 0.5);
            font-size: 0px;
            .avatar {
                display: inline-block;
                vertical-align: top;
                margin-right: 16px;
            }
            .content {
                display: inline-block;
                vertical-align: top;
                .title {
                    margin-top: 2px;
                    .brand {
                        display: inline-block;
                        width: 30px;
                        height: 18px;
                        background-image: url("brand@2x.png");
                        background-size: cover;
                        vertical-align: middle;
                    }
                    .name {
                        display: inline-block;
                        vertical-align: middle;
                        font-size: 16px;
                        line-height: 18px;
                        color: rgb(255, 255, 255);
                        font-weight: bold;
                    }
                }
                .desc {
                    margin-top: 8px;
                    margin-bottom: 10px;
                    font-size: 12px;
                    line-height: 12px;
                    color: rgb(255, 255, 255);
                    font-weight: 200;
                }
                .supports {
                    .icon {
                        display: inline-block;
                        margin-right: 4px;
                        width: 12px;
                        height: 12px;
                        background-size: cover;
                        background-repeat: no-repeat;
                        vertical-align: middle;
                        &.decrease {
                            background-image: url("./decrease_1@2x.png");
                        }
                        &.discount {
                            background-image: url("./discount_1@2x.png");
                        }
                        &.guarantee {
                            background-image: url("./guarantee_1@2x.png");
                        }
                        &.invoice {
                            background-image: url("./invoice_1@2x.png");
                        }
                        &.special {
                            background-image: url("./special_1@2x.png");
                        }

                    }
                    .text {
                        display: inline-block;
                        color: #fff;
                        font-size: 10px;
                        line-height: 12px;
                        font-weight: 200;
                        vertical-align: middle;
                    }

                }
            }
            .support-count {
                position: absolute;
                right: 12px;
                bottom: 18px;
                color: #fff;
                font-size: 10px;
                line-height: 12px;
                background: rgba(0, 0, 0, 0.2);
                padding: 7px 8px;
                border-radius: 14px;
            }
        }
        .bulletin-wrapper {
            height: 28px;
            line-height: 28px;
            padding: 0 36px 0 12px;
            box-sizing: border-box;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            background-color: rgba(7, 17, 27, 0.6);
            color: #fff;
            position: relative;
            .icon {
                display: inline-block;
                vertical-align: top;
                width: 30px;
                height: 18px;
                margin-top: 5px;
                background-image: url("./bulletin@2x.png");
                background-size: cover;
            }
            .text {
                display: inline-block;
                vertical-align: top;
                font-size: 10px;
            }
            .icon-keyboard_arrow_right {
                position: absolute;
                right: 0;
                top: 8px;
            }
        }
        .background {
            position: absolute;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            filter: blur(10px);

        }
        .detail {
            position: fixed;
            z-index: 100;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background: rgba(7, 17, 27, 0.8);
            display: flex;
            flex-flow: column;
            overflow: auto;
            transition: opacity 0.5s;
            &.fade-enter, &.fade-leave-to {
                opacity: 0;
            }
            .detail-wrapper {
                flex: 1;
                overflow: auto;
                margin-top: 64px;
                .title-wrapper {
                    text-align: center;
                    .title {
                        font-size: 16px;
                        line-height: 16px;
                        color: #fff;
                        font-weight: 700;
                        margin-bottom: 16px;
                    }
                    .el-rate__icon {
                        font-size: 24px;
                        margin-right: 15px;
                    }
                }
                .separate {
                    margin-top: 28px;
                    box-sizing: border-box;
                    padding: 0 36px;
                    display: flex;
                    .line {
                        flex: 1;
                        border-bottom: 1px solid rgba(255, 255, 255, 0.2);
                        position: relative;
                        top: -6px;
                    }
                    .title {
                        font-size: 14px;
                        line-height: 14px;
                        font-weight: 700;
                        color: #fff;
                        margin: 0 12px;
                    }
                }
            }
            .detail-close {
                text-align: center;
                font-size: 32px;
                color: rgba(255, 255, 255, 0.5);
                padding-bottom: 32px;
            }

        }
    }

</style>
