<template>
  <div class="goods">
    <!--分类列表-->
    <div class="menu-wrapper" ref="menuScroll">
      <ul>
        <!--专场,专场特殊,需单独做-->
        <li class="menu-item" :class="{'current':currentIndex === 0}" @click="selectMenu(0)">

          <p class="text">
            <img class="icon" :src="list_first.tag_icon" v-if="list_first.tag_icon" />
            {{list_first.tag_name}}
          </p>
        </li>
        <li class="menu-item" :class="{'current':currentIndex === index + 1}" v-for="(item,index) in list_con" :key="index"
          @click="selectMenu(index+1)">
          <p class="text">
            <img class="icon" :src="item.icon" v-if="item.icon" />
            {{item.name}}
          </p>
          <!--数字-->
          <i class="num" v-show="calculateCount(item.spus)">
            {{calculateCount(item.spus)}}
          </i>
        </li>
      </ul>
    </div>
    <!--商品列表-->
    <div class="foods-wrapper" ref="foodScroll">
      <ul>
        <!--专场菜单,对应的只有两张图片-->
        <li class="container-list food-list-hook">
          <div v-for="(item,index) in list_first.operation_source_list" :key="index">
            <img :src="item.pic_url" />
          </div>
        </li>
        <!--具体分类-->
        <li v-for="(item,index) in list_con" :key="index" class="food-list food-list-hook">
          <h3 class="title">{{item.name}}</h3>
          <!--具体商品列表-->
          <ul>
            <!--  -->
            <li v-for="(food,index) in item.spus" :key="index" @click="showDetail(food)" class="food-item">
              <div class="icon" :style="head_bg(food.picture)"></div>
              <div class="content">
                <h3 class="name">{{food.name}}</h3>
                <p class="desc" v-if="food.description">{{food.description}}</p>
                <div class="extra">
                  <span class="saled">{{food.month_saled_content}}</span>
                  <span class="saled">{{food.praise_content}}</span>
                </div>
                <img class="product" :src="food.product_label_picture" v-if="food.product_label_picture" />
                <p class="price">
                  <span class="text">￥{{food.min_price}}</span>
                  <span class="unit">/{{food.unit}}</span>
                </p>
              </div>
              <app-cart-control :food="food"></app-cart-control>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <!--购物车-->
    <ShopCart :poiInfo="poiInfo" :selectFoods="selectFoods"></ShopCart>

    <!--商品详情-->
    <productDetail :food="selectFood" ref="foodView"></productDetail>
  </div>
</template>

<script>
  import axios from "axios"
  import BScroll from 'better-scroll'
  import productDetail from "../productDetail/productDetail.vue"
  import ShopCart from "../shopCart/shopCart.vue"
  import cartControl from "../cartControl/cartControl.vue"
  export default {
    name: 'HelloWorld',
    components: {
      "productDetail": productDetail,
      'ShopCart': ShopCart,
      "app-cart-control": cartControl
    },
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        list_con: [],
        list_first: {},
        scroll_con_y: 0,
        selectFood: {},
        poiInfo: {},
        arr_heigh: [],
        total_Y: 0
      }
    },
    computed: {
      currentIndex() {
        // arr.forEach((res)=>{
        //   nub+=res.clientHeight
        //   this.arr_heigh.push(nub);
        // })为什么不行？
        // console.log(this.arr_heigh);
        for (let i = 0; i < this.arr_heigh.length; i++) {
          if (this.scroll_con_y >= this.arr_heigh[i] && this.scroll_con_y < this.arr_heigh[i + 1] || !this.arr_heigh[i +
              1]) {
            return i
          }
        }
      },
      selectFoods() {
        let count_h = [];
        this.list_con.forEach((res) => {
          res.spus.forEach((pos) => {
            if (pos.count > 0) {
              count_h.push(pos);
            }

          })
        })
        return count_h
      }
    },
    methods: {
      selectMenu(number) {
        let arr = this.$refs.foodScroll.getElementsByClassName('food-list-hook');
        this.foodScroll.scrollToElement(arr[number], 500);
        console.log(number);
        console.log(this.arr_heigh)
        let shu = 0;
        for (let i = 0; i < number; i++) {
          shu += this.arr_heigh[i]
        }
        console.log(shu);
        this.total_Y = shu;

      },
      //背景图片
      head_bg(address) {
        return "background-image: url(" + address + ") ;"
      },
      //打开详情页
      showDetail(food) {
        this.selectFood = food;
        this.$refs.foodView.showView();
        // console.log(this.selectFood)
      },
      //小图标
      calculateCount(pos) {
        let total = 0;
        pos.forEach((res) => {
          if (res.count) {
            total += res.count;
          }
        })
        return total
      }
    }, //引入图片
    created() {
      //获取数据和引入滑动
      axios.get('/goods').then((res) => {
        this.list_con = res.data.data.food_spu_tags;
        this.poiInfo = res.data.data.poi_info;
        console.log(res.data)
        this.list_first = res.data.data.container_operation_source;
        this.$nextTick(() => {
          //获取arr_heigh
          let arr = document.getElementsByClassName('food-list-hook');
          let nub = 0;
          this.arr_heigh.push(nub);
          for (let i = 0; i < arr.length; i++) {
            let item = arr[i];
            nub += item.clientHeight;
            this.arr_heigh.push(nub);
          }////
          var onwheel = (e) => { //鼠标滚动
            this.total_Y += e.deltaY;
            console.log(e.deltaY);
            console.log(this.total_Y);
            var max_scroll = this.arr_heigh[this.arr_heigh.length - 1];
            console.log(max_scroll);
            if (this.total_Y <= 0) {
              this.total_Y = 0;
            } else if (this.total_Y >= max_scroll) {
              this.total_Y = max_scroll;
            }
            this.foodScroll.scrollTo(0, -this.total_Y);
          };
          this.$refs.foodScroll.addEventListener("wheel", onwheel );////
          //两个列表的滑动
          if (!this.menuScroll) {
            this.menuScroll = new BScroll(this.$refs.menuScroll, {
              click: true
            })
          } else {
            this.menuScroll.refresh();
          };
          if (!this.foodScroll) {
            this.foodScroll = new BScroll(this.$refs.foodScroll, {
              click: true,
              probeType: 3
            })
          } else {
            this.foodScroll.refresh();
          };////
          //监听滑动
          this.foodScroll.on('scroll', (res) => {
            this.scroll_con_y = Math.abs(Math.round(res.y));
          });////
        })
      })
    },
    mounted() {

    }

  }
</script>

<style scoped>
  @import url("./goods.css");
</style>
