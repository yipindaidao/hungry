<template>
    <div class="goods">
        <div class="menu-wrapper">
            <ul class="menu">
                <li class="menu-item" v-for="(item,index) in goods" :class="{current: currentIndex === index}" @click="selectMenu(index,$event)">
                    <span class="text">{{ item.name }}</span>
                </li>
            </ul>
        </div>
        <div class="goods-wrapper" ref="goodswrapper">
            <ul>
                <li v-for="item in goods" class="good-item good-item-hook">
                    <h1 class="title">{{ item.name }}</h1>
                    <ul class="foods">
                        <li v-for="food in item.foods" class="food-item" @click="gotoFood(food)">
                            <div class="icon">
                                <img width="57" height="57" :src="food.icon"/>
                            </div>
                            <div class="content">
                                <h2 class="name">{{ food.name }}</h2>
                                <p class="desc">{{ food.description }}</p>
                                <p class="sellCount"><span>月售{{ food.sellCount }}份</span> <span>好评率{{ food.rating
                                    }}%</span></p>
                                <p class="price">
                                    <span class="nowPrice">￥{{ food.price }}</span><span class="oldPrice"
                                                                                         v-show="food.oldPrice">￥{{ food.oldPrice
                                    }}</span></p>
                                <div class="cartcontrol-wrapper">
                                    <cartcontrol :selectfood="food" @cartAddEvent="cartAdd"></cartcontrol>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart ref="shopcart" :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
        <food :selfood="selectedfood" ref="fd"></food>
    </div>
</template>

<script>
    import BScroll from 'better-scroll'
    import shopcart from '../shopcart/shopcart'
    import cartcontrol from '../cartcontrol/cartcontrol'
    import food from '../food/food'
    export default {
        props: {
            seller: {
                type: Object
            }
        },
        data() {
            return {
                goods: [],
                selectedfood: {},
                listHeight: [],
                scrollY: 0
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
                    this.menuscroll = new BScroll('.menu-wrapper',{
                        click: true
                    })
                    this.goodsscroll = new BScroll('.goods-wrapper',{
                        click: true,
                        probeType: 3
                    })
                    this.goodsscroll.on('scroll',(pos) => {
                        this.scrollY = Math.abs(Math.round(pos.y))
                    })
                    this.calculateHeight()
                })
            },
            calculateHeight() {
                let foodsList = this.$refs.goodswrapper.getElementsByClassName('good-item-hook')
                let height = 0
                this.listHeight.push(height)
                for(let i=0;i<foodsList.length;i++) {
                    let foods = foodsList[i]
                    height += foods.clientHeight
                    this.listHeight.push(height)
                }
            },
            selectMenu(index,event) {
                if(!event._constructed) {
                    return
                }
                let food = this.$refs.goodswrapper.getElementsByClassName('good-item-hook')[index]
                this.goodsscroll.scrollToElement(food,300)
            },
            cartAdd(target) {
                this.$refs.shopcart.drop(target)
            },
            gotoFood(food) {
                this.selectedfood = food
                this.$refs.fd.show()
            }
        },
        computed: {
            currentIndex() {
                for(let i=0;i<this.listHeight.length;i++) {
                    let f = this.listHeight[i]
                    let n = this.listHeight[i+1]
                    if(!n || (this.scrollY >= f && this.scrollY < n)) {
                        return i
                    }
                }
                return 0
            },
            selectFoods() {
                let foods = []
                this.goods.forEach(good => {
                    good.foods.forEach(food => {
                        if(food.count) {
                            foods.push(food)
                        }
                    })
                })
                return foods
            }
        },
        components: {
            shopcart,cartcontrol,food
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
                    width: 80px;
                    padding-left: 10px;
                    height: 54px;
                    line-height: 14px;
                    display: table;
                    &.current {
                        background: #fff;
                        font-weight: 700;
                    }
                    .text {
                        display: table-cell;
                        vertical-align: middle;
                        font-size: 12px;
                        color: rgb(77, 85, 93);
                        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
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
                    color: rgb(147, 153, 159);
                    padding-left: 14px;
                }
                .foods {
                    .food-item {
                        margin: 0 18px;
                        padding: 18px 0;
                        display: flex;
                        border-bottom: 1px solid rgba(7,17,27,0.1);
                        .icon {
                            width: 57px;
                            margin-right: 10px;
                        }
                        .content {
                            flex: 1;
                            position: relative;
                            .name {
                                font-size: 14px;
                                line-height: 14px;
                                color: rgb(7, 17, 27);
                            }
                            .desc {
                                font-size: 10px;
                                line-height: 10px;
                                color: rgb(147, 153, 159);
                                margin: 8px 0;
                            }
                            .sellCount {
                                font-size: 10px;
                                line-height: 10px;
                                color: rgb(147, 153, 159);
                                margin-bottom: 8px;
                            }
                            .price {
                                .nowPrice {
                                    font-size: 14px;
                                    color: rgb(240, 20, 20);
                                    line-height: 14px;
                                    font-weight: 700;
                                    margin-right: 8px;
                                }
                                .oldPrice {
                                    font-size: 10px;
                                    color: rgb(147, 153, 159);
                                    line-height: 14px;
                                    font-weight: 700;
                                }
                            }
                            .cartcontrol-wrapper {
                                position: absolute;
                                right: 0;
                                bottom: -12px;
                            }
                        }
                    }
                }
            }
        }
    }


</style>
