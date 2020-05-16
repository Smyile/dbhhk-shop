<template>
 <div class="nav">
   <!--tab选项卡式的nav-->
   <router-link to="/goods" class="nav-item">
   	商品
   	<i class="line"></i>
   </router-link>
   <router-link to="/ratings" class="nav-item">
   	评价<span style="font-size: 15px;">({{commentNum}})</span>
   	<i class="line"></i>
   </router-link>
   <router-link to="/seller" class="nav-item">
   	商家
   	<i class="line"></i>
   </router-link>
 </div>
</template>

<script>
  import Goods from '../goods/goods.vue'
  import Ratings from '../ratings/ratings.vue'
  import Seller from '../seller/seller.vue'
  export default {
    components: {
      // Goods: Goods,
      // Ratings: Ratings,
      // Seller: Seller

    },
    props: {
      commentNum: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        activeName: 'second'
      };
    },
    methods: {
      handleClick(tab, event) {
        console.log(tab, event);
        this.activeName=tab.name;
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
    z-index: 233399;
    margin-top: 5px;
    background:rgba(0, 54, 54, 0.5);
  }
  .nav:hover{
    border-bottom: 2px solid #656565;
  }

  .nav-item {
    flex: 1;
    text-align: center;
    font-size: 20px;
    height: 100%;
    line-height: 50px;
    /* border-radius: 20px; */
    text-decoration: none;
    color: #fff;
    position: relative;
    opacity: 1;
    top: 0px;
  }

  /*选中的样式*/
  .router-link-active {
    color: deepskyblue;
    border-bottom: 2px solid deepskyblue;
    /* text-shadow: 2px 2px 2px #7F7F7F; */
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
