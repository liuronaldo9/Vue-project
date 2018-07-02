<template>
    <transition name="fade">
        <div class="popup-wrapper" v-show="showPop">
            <div class="choice-wrapper">
                <h3>{{food.name}}
                    <a @click="closeSelect">
                        <i class="icon-close"></i>
                    </a>
                </h3>
                <div class="choice-detail">
                    <div class="select-wrapper">
                        <h4 class="select-title">面类(必选)</h4>
                        <div class="selections">
                            <span @click="selectSpec" :class="{'select-active': sleceted}">刀削面</span>
                            <span>刀削面</span>
                            <span>刀削面</span>
                        </div>
                    </div>
                </div>
                <div class="choice-bottom">
                    <p class="select-price">
                        <span>$11</span>
                    </p>
                    <button class="select-button">加入购物车</button>
                </div>
            </div> 
        </div>
    </transition>
</template>

<script>
export default {
    props: {
            food: {
                type: Object
            }
    },
    data() {
        return {
            showPop: false,
            sleceted: false
        }
    },
    methods: {
        popup() {
            this.showPop = true
        },
        selectSpec() {
            this.sleceted = !this.sleceted
        },
        closeSelect() {
            this.showPop = false
        }
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"
.popup-wrapper
    position: fixed
    z-index: 100
    top: 0
    left: 0
    height: 100%
    width: 100%
    overflow: hidden
    opacity: 1  
    backdrop-filter: blur(10px)
    background: rgba(7,17,27,0.8)
    &.fade-enter-active, &.fade-leave-active
        transition: all 0.5s
    &.fade-enter, &.fade-leave-active
        opacity: 0
        background: rgba(7, 17, 27, 0)
    .choice-wrapper
        position: fixed
        z-index: 101
        top: 40%
        left: 10%   
        right: 10%  
        overflow: auto
        border-radius: 5px
        background-color: #fff
        h3
            width: 80%
            font-weight: 400
            text-align: center
            margin: 8px auto
            font-size: 18px
            a
                position: absolute
                right: 10px
                font-size: 25px
                top: 4px
        .choice-detail
            padding: 5px 1px;
            padding-bottom: 0;
            font-size: 13px;
            .select-title, .selections   
                margin-left: 10px
                margin-bottom: 10px 
                span 
                    display: inline-block
                    color: #666
                    border: 1px solid #999
                    padding: 5px 5px;
                    border-radius: 1rem
                    margin-right: 5px
                    margin-bottom: 5px
                    &.select-active
                        color: #3199e8
                        border-color: #3199e8 
        .choice-bottom
            display: flex
            align-items: center
            border-top: 1px solid #eee
            border-bottom: 1px solid #eee
            background-color: #f9f9f9
            padding: .8rem 1rem
            margin-top: 1rem
            .select-price
                margin: 0
                color: #f60
                line-height: 1.2rem
                font-size: 1.2rem
                font-weight: 700
            .select-button
                border-radius: 4px
                border: 0
                background-color: #26a2ff
                color: #ffffff
                font-size: 15px
                left: 60%
                padding: 5px 12px
                display: inline-block
                position: relative
                text-align: center
</style>
