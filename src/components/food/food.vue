<template>
    <transition name="show">
        <div class="food" v-show="showflag">
            <div class="image">
                <img :src="selfood.image"/>
                <i class="icon-arrow_lift" @click="hide"></i>
            </div>
            <div class="basic"></div>
            <div class="seperate"></div>
            <div class="desc"></div>
            <div class="seperate"></div>
            <div class="rating">
            </div>
        </div>
    </transition>
</template>

<script>
    export default {
        props: {
            selfood: {}
        },
        data() {
            return {
                showflag: false
            }
        },
        methods: {
            show() {
                this.showflag = true
            },
            hide() {
                this.showflag = false
            }
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

    }

</style>
