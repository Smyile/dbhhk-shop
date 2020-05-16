<template>
  <div class="shopcart">
    <div class="shopcart-wrapper">
      <!--左侧部分-->
      <div class="content-left">
        <!--logo部分-->
        <div class="logo-wrapper" :class="{'highligh':totalCount>0}">
          <span class="fa fa-shopping-cart logo" :class="{'highligh':totalCount>0}" @click="toggleList"></span>
          <i class="num" v-show="totalCount">{{totalCount}}</i>
        </div>
        <div class="desc-wrapper">
          <p class="total-price">
            ￥{{totalPrice}}
          </p>
          <p class="tip" :class="{'highligh':totalCount>0}">
            另需:{{poiInfo.shipping_fee_tip}}
          </p>
        </div>
      </div>
      <!--右侧部分-->
      <div class="content-right" :class="{'highligh':totalCount>0}" @mouseenter="f_number"  @mouseleave="f_number">
        <span v-show="r_number">{{payStr}}</span>
        <span v-show="!r_number">去结算</span>
      </div>

      <!--购物车列表-->
      <div class="shopcart-list" v-show="listShow" :class="{'show':listShow}">
        <div class="list-top" v-if="poiInfo.discounts2">
          {{poiInfo.discounts2[0].info}}
        </div>
        <div class="list-header">
          <h3 class="title">1号口袋</h3>
          <div class="empty" @click="clearAll">
            <img src="./img/ash_bin.png" />
            <span>清空购物车</span>
          </div>
        </div>
        <div class="list-content" ref="listContent">

          <ul>
            <li class="food-item" v-for="(food,index) in selectFoods" :key="index">
              <div class="desc-wrapper">
                <div class="desc-left">
                  <p class="name">{{food.name}}</p>
                  <p class="unit" v-show="!food.description">{{food.unit}}</p>
                  <p class="description" v-show="!food.unit">{{food.description}}</p>
                </div>
                <div class="desc-right">
                  ￥{{food.min_price}}
                </div>
              </div>
              <div class="cartcontrol-wrapper">
                <!--加减号控件-->
                <app-cart-control :food="food"></app-cart-control>
              </div>
            </li>
          </ul>

        </div>
        <div class="list-bottom"></div>
      </div>

    </div>
    <div class="shopcart-mask" v-show="listShow" @click="toggleList"></div>
  </div>
</template>

<script>
  import CartControl from "../cartControl/cartControl"
  import BScroll from 'better-scroll'
  import control from "../cartControl/cartControl.vue"
  export default {
    components: {
    "app-cart-control":control
    },
    data() {
      return {
        listShow:false,
        r_number:true
      }
    },

    props: {
      selectFoods:{
        type:Array,
        default(){
          return []
        }
      },
      poiInfo:{
        type:Object,
        default:{}
      }

    },
    computed: {
      //求购物车数量和
      totalCount(){
        let count_h=0;
        this.selectFoods.forEach((res)=>{
          count_h+=res.count;
        })
        if(count_h==0){
          this.listShow=false;
        }
        return count_h
      },
      //求购物车物品总价
      totalPrice(){
        let count_h=0;
        this.selectFoods.forEach((res)=>{
          count_h+=res.count*res.min_price;
        })
        return count_h
      },
      //加上配送费
      payStr(){
        if(!this.totalCount){
          return
        }
        let total= this.totalPrice+9
      	return  "合计：" +total+"元"
      }
    },
    methods: {
      f_number(){
        if(this.totalCount==0){
          return
        }
        this.r_number=!this.r_number;
      },
      toggleList(){
        if(this.totalCount==0){
          return
        }else{
          this.listShow=!this.listShow
          if(this.listShow){
          	//dom渲染完实现滚动
          	this.$nextTick(()=>{
          		//开始shopScroll不存在实现一个
          		if(!this.shopScroll){
          			this.shopScroll=new BScroll(this.$refs.listContent,{
          				click:true
          			})
          		}else{
          			//已经发生变化刷新
          			this.shopScroll.refresh();
          		}
          	})
          }
        }
      },
      clearAll(){
        this.listShow=false
        this.selectFoods.forEach((res)=>{
          res.count=0;
        })
      }
    },
  }
</script>

<style scoped>
  @import url("../../common/css/font-awesome.css");
  @import url("./shopCart.css");
</style>
