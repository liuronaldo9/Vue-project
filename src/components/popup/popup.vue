<template>
    <transition name="fade">
        <div class="detail" v-show="showPop">
            <div class="detail-wrapper clearfix">
                <h3>{{food.name}}</h3>
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
            showPop: false
        }
    },
    methods: {
        popup() {
            this.showPop = true
        }
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"
.detail
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
    h3
        color: #ffffff
</style>
