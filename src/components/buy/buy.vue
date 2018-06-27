<template>
    <div class="buy">
        <span class="buyButton" @click.stop.prevent="selectSize">
        选规格
      </span>
    </div>
</template>

<script>
export default {
    methods: {
        selectSize: function(event) {
            console.log('sss')
        }
    }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
.buyButton
    position: absolute
    right: 10px
    bottom: 2px
    padding: 5px 5px
    width: 40px
    text-align: center
    border-radius: 12px
    font-size: 10px
    color: #fff
    background: rgb(0, 160, 220)
</style>    