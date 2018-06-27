<template>
<div>
    <div class="spec">
        <span class="specButton">
        选规格
      </span>
    </div>
</div>
</template>

<script>
export default {
    props: {
        food: {
            type: Object
        }
    }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
.specButton
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