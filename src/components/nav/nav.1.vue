<template>
  <div class="nav">
    <!-- <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="商品" name="goods">
        <Goods></Goods>
      </el-tab-pane>
      <el-tab-pane label="评论" name="ratings">
        <Ratings></Ratings>
      </el-tab-pane>
      <el-tab-pane label="商家" name="third">
        <Seller></Seller>
      </el-tab-pane>
      <el-tab-pane label="定时任务补偿" name="fourth">定时任务补偿</el-tab-pane>
    </el-tabs> -->
     <!--tab选项卡式的nav-->
     <div class="nav">
       <!--tab选项卡式的nav-->
       <router-link to="/goods" class="nav-item">
       	商品
       	<i class="line"></i>
       </router-link>
       <router-link to="/ratings" class="nav-item">
       	评价
         ({{commentNum}})
       	<i class="line"></i>
       </router-link>
       <router-link to="/seller" class="nav-item">
       	商家
       	<i class="line"></i>
       </router-link>
     </div>
  </div>
</template>

<script>
  import Goods from '../goods/goods.vue'
  import Ratings from '../ratings/ratings.vue'
  import Seller from '../seller/seller.vue'
  export default {
    components:{
      Goods:Goods,
      Ratings:Ratings,
      Seller:Seller

    },
    props: {
      commentNum: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {

      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .nav {
    display: flex;
    width: 100%;
    height: 50px;
    line-height: 50px;
    margin-top: 10px;
    /* background-color: #FFD161; */
  }

  .nav-item {
    flex: 1;
    text-align: center;
    font-size: 20px;
    height: 35px;
    line-height: 35px;
    background-color: #CDCDCC;
    border-radius: 20px;
    text-decoration: none;
    color: #666;
    position: relative;
    margin: 3px;
    top: 0px;
    border-bottom: 1px solid #e4e4e4;
    opacity: 0.6;
  }

  .nav-item:hover {
    opacity: 1;
  }

  /*选中的样式*/
  .router-link-active {
    color: darkolivegreen;
    text-shadow: 2px 2px 2px #7F7F7F;
  }

  .nav .active .line {
    width: 20px;
    height: 2px;
    display: inline-block;
    background-color: #ffbb22;
    position: absolute;
    left: 50%;
    bottom: 0;
    margin-left: -10px;
  }
</style>
