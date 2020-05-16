<template>
  <div class="header">
    <!--顶部通栏搜索框这里开始-->
    <div class="top-wrapper">

      <div class="back-wrapper">
        <span class="arrow-circle-left fa fa-arrow-circle-left"></span>

      </div>

      <form class="search-wrapper">
        <span class="search-icon"></span>
        <input class="search-bar" type="text" placeholder="搜索店内商品" />
      </form>

      <div class="more-wrapper">
        <a class="spelling-bt" href="#">拼单</a>
        <!-- <div class="more-bt"> -->
        <el-dropdown>
          <el-button >
            更多~
          </el-button>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>smyile</el-dropdown-item>
            <el-dropdown-item>vip</el-dropdown-item>
            <el-dropdown-item>happy</el-dropdown-item>
            <el-dropdown-item>funny</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <!-- </div> -->
      </div>
    </div>
    <!--顶部通栏结束-->

    <!-- 主题内容 开始 -->
    <div class="content-wrapper">
      <div class="icon" :style="head_bg">
        <!-- <img :src="Hder_pinfor.pic_url" /> -->
      </div>
      <div class="name">
        <h3>{{Hder_pinfor.name}}</h3>
      </div>
      <div class="collect" @click="F_collect">
        <span  v-show="Hder_collect" ><i class="el-icon-star-off"></i>收藏</span>
        <span  v-show="!Hder_collect" ><i class="el-icon-star-on" style="color: goldenrod;"></i>已收藏</span>
      </div>
    </div>

    <!-- 主题内容 结束 -->

    <!-- <! 公告内容 开始 -->
    <div class="bulletin-wrapper">
      <img class="icon" v-if="Hder_pinfor.discounts2" :src="Hder_pinfor.discounts2[0].icon_url" />

      <span class="text" v-if="Hder_pinfor.discounts2">{{Hder_pinfor.discounts2[0].info}}</span>
      <div class="detail" v-if="Hder_pinfor.discounts2" @click="Hder_g_btm">
        {{Hder_pinfor.discounts2.length}}个活动
        <span class="icon-keyboard_arrow_right"></span>
      </div>
    </div>
    <!-- 公告内容 结束 -->

    <!--公告详情开始-->
    <div class="bulletin-detail" v-show="parameter_gg">
      <div class="detail-wrapper">
        <!--相关容器-->
        <div class="main-wrapper" :style="detail_bg">
          <div class="icon" :style="head_bg"></div>
          <h3 class="name">{{Hder_pinfor.name}}</h3>

          <!-- 星级评价 -->
          <div class="score">
            <!-- <app-star :score="Hder_pinfor.wm_poi_score"></app-star> -->
            <span>{{Hder_pinfor.wm_poi_score}}</span>
          </div>


          <p class="tip">
            {{Hder_pinfor.min_price_tip}} <i>|</i> {{Hder_pinfor.shipping_fee_tip}} <i>|</i>
            {{Hder_pinfor.delivery_time_tip}}
          </p>

          <p class="time">
            配送时间: {{Hder_pinfor.shipping_time}}
          </p>

          <div class="discounts" v-if="Hder_pinfor.discounts2">
            <p>
              <img :src="Hder_pinfor.discounts2[0].icon_url" />
              <span>{{Hder_pinfor.discounts2[0].info}}</span>
            </p>
          </div>

        </div>
        <!--关闭内容容器-->
        <div class="close-wrapper">
          <span class="icon-close" @click="Hder_g_btm">&times;</span>
        </div>
      </div>
    </div>
    <!--公告详情结束-->

    <!-- 背景内容 开始 -->
    <div class="bg-wrapper" :style='head_pic_url'>

    </div>

    <!-- 背景内容 结束 -->
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        parameter_gg: false,
        Hder_collect:true
      }
    },
    props: {
      Hder_pinfor: {
        type: Object,
        default () {
          return
        }
      }
    },
    methods: {
      Hder_g_btm() {
        this.parameter_gg = !this.parameter_gg
      },
      F_collect(){
        this.Hder_collect = !this.Hder_collect
      }
    },
    computed: {
      head_bg() {
        return "background-image: url(" + this.Hder_pinfor.pic_url + ");"
        // return "background-image: url(" + this.poiInfo.head_pic_url + ");"
      },
      detail_bg() {
        return "background-image: url(" + this.Hder_pinfor.poi_back_pic_url + ");"
      },

      head_pic_url() {
        return "background-image: url(" + this.Hder_pinfor.head_pic_url + ");"
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style>
  @import url("../../common/css/font-awesome.css");
  @import './header.css'
</style>
