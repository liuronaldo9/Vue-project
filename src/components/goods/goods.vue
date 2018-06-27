<template>
<div>
    <div class="goods">
        <div class="menu-wrapper" ref="menuWrapper">
            <ul>
                <li v-for="(item, index) of goods" :key="index" class="menu-item" ref="menuList" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)">
                    <span class="text border-1px">
                        <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
                        {{item.name}}
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-wrapper" ref="foodsWrapper">
            <ul>
                <li v-for="(item, index) of goods" :key="index" class="food-list" ref="foodList">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li @click="selectFood(food, $event)" v-for="(food, index) of item.foods" :key="index" class="food-item border-1px">
                            <div class="icon">
                                <img :src="food.icon" width="57px" height="57px">
                            </div>
                            <div class="content">
                                <h2 class="name">{{food.name}}</h2>
                                <p class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span class="count">月售{{food.sellCount}}份</span>
                                    <span>好评率{{food.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="now">${{food.price}}</span>
                                    <span class="old" v-show="food.oldPrice">${{food.oldPrice}}</span>
                                </div>
                                <div class="cartcontrol-wrapper">
                                    <v-buy v-show="food.spec_cats"></v-buy>
                                    <v-cartcontrol  @add="addFood" :food="food" v-show="!food.spec_cats"></v-cartcontrol>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <v-shopCart   ref="shopCart" :selectFoods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></v-shopCart>
    </div>
    <v-food :food="selectedFood" ref="food"></v-food>
</div>
</template>

<script>
import BScroll from 'better-scroll';
import shopCart from './../shopCart/shopCart';
import cartcontrol from './../cartcontorl/cartcontorl';
import food from './../food/food';
import buy from './../buy/buy';



const ERR_OK = 0;
export default {
    props: {
        seller: {
            type: Object
        }
    },
    components: {
       'v-shopCart':  shopCart,
       'v-cartcontrol': cartcontrol,
       'v-food': food,
       'v-buy': buy
    },
    data() {
        return {
            goods: [],
            listHeight: [],
            scrollY: 0,
            selectedFood: {}
        }
    },
    created() {
        this.$http.get('./api/goods').then((result) => {
      result = result.body;
      if(result.errno === ERR_OK){
        this.goods = result.data
        console.log(this.goods)
        this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
        });
      }
    }).catch((err) => {
      
    });
    this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special' ]
    },
    computed: {
        currentIndex() {
            for (let i = 0; i < this.listHeight.length; i++) {
                let height1 = this.listHeight[i];
                let height2 = this.listHeight[i + 1];
                if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                    this._followScroll(i);
                    return i;
                } 
            }
                return 0;
        },
        selectFoods() {
            let foods = [];
            this.goods.forEach((good) => {
                good.foods.forEach((food) => {
                    if(food.count){
                        foods.push(food);
                    }
                });
            });
            return foods;
        }
    },
    methods: {
        selectSzie: function() {
            console.log("sssss")
        },
        _initScroll: function() {
            this.meunScroll = new BScroll(this.$refs.menuWrapper, {
                click: true
            });

            this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
                probeType: 3,
                click: true
            });
            this.foodsScroll.on('scroll', (pos) => {
              if (pos.y <= 0) {
                 this.scrollY = Math.abs(Math.round(pos.y));
                }
            });
        },
        _calculateHeight: function() {
            let foodList = this.$refs.foodList;
            // console.log(foodList)
            let height = 0;
            this.listHeight.push(height);
            for(let i=0; i<foodList.length; i++){
                let item = foodList[i];
                height += item.clientHeight;
                this.listHeight.push(height);
            }
        },
        selectMenu: function(index, event) {
            if (!event._constructed) {
                return;
            }
            let foodList = this.$refs.foodList;
            let el = foodList[index];
            this.foodsScroll.scrollToElement(el, 300);
        },
        addFood(target) {
        this._drop(target);
      },
      _drop(target) {
        this.$nextTick(() => {
          this.$refs.shopCart.drop(target);
        });
      },
      selectFood(food, event) {
          if(!event._constructed) {
              return
          }
          this.selectedFood = food;
          this.$refs.food.show();
      },
      _followScroll(index) {
          let menuList = this.$refs.menuList;
          let el = menuList[index];
          this.meunScroll.scrollToElement(el, 300, 0, -100);
      }
    }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/mixin.styl"
.goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
        flex: 0 0 80px
        width: 80px
        background: #f3f5f7
        .menu-item
            display: table
            height: 54px
            width: 56px
            padding: 0 12px
            line-height: 14px
            &.current
                position: relative
                z-index: 10
                margin-top: -1px
                background: #fff
                font-weight: 700
                .text
                    border-none()
            .icon
                display: inline-block
                vertical-align: top
                width: 12px
                height: 12px
                margin-right: 2px
                background-size: 12px 12px
                background-repeat: no-repeat
                &.decrease
                    bg-image('decrease_3')
                &.discount
                    bg-image('discount_3')
                &.guarantee
                    bg-image('guarantee_3')
                &.invoice
                    bg-image('invoice_3')
                &.special
                    bg-image('special_3')
            .text
                display: table-cell
                width: 56px
                vertical-align: middle
                border-1px(rgba(7, 17, 27, 0.1))
                font-size: 12px
     .foods-wrapper
            flex: 1
            .title
                padding-left: 14px
                height: 26px
                line-height: 26px
                border-left: 2px solid #d9dde1
                font-size: 12px
                color: rgb(147, 153, 159)
                background: #f3f5f7
            .food-item
                display: flex
                margin: 18px
                padding-bottom: 18px
                border-1px(rgba(7, 17, 27, 0.1))
                &:last-child
                    border-none()
                    margin-bottom: 0
                .icon
                    flex: 0 0 57px
                    margin-right: 10px
                .content
                    flex: 1
                    .name
                        margin: 2px 0 8px 0
                        height: 14px
                        line-height: 14px
                        font-size: 14px
                        color: rgb(7, 17, 27)
                    .desc, .extra
                        line-height: 10px
                        font-size: 10px
                        color: rgb(147, 153, 159)
                    .desc
                        line-height: 12px
                        margin-bottom: 8px
                    .extra
                        margin-top: 20px
                        .count
                            margin-right: 12px
                    .price
                        font-weight: 700
                        line-height: 24px
                        .now
                            margin-right: 8px
                            font-size: 14px
                            color: rgb(240, 20, 20)
                        .old
                            text-decoration: line-through
                            font-size: 10px
                            color: rgb(147, 153, 159)
                    .cartcontrol-wrapper
                        position: absolute
                        right: 0
                        bottom: 12px
</style>