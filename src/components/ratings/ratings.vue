<template>
  <div class="ratings" ref="ratingView">
    <div class="ratings-wrapper">
      <!--评价-->
      <div class="overview">
        <div class="overview-left">
          <!--左侧部分开始-->
          <!--商家评分-->
          <div class="comment-score">
            <p class="score">{{ratings.comment_score}}</p>
            <p class="text">商家评分</p>
          </div>
          <!--口味和包装-->
          <div class="other-score">
            <div class="quality-score item">
              <span class="text">口味</span>
              <Star :score="ratings.quality_score" class='star'></Star>
              <span class="score"></span>
            </div>
            <div class="pack-score item">
              <span class="text">包装</span>
              <Star :score="ratings.pack_score" class='star'></Star>
              <span class="score"></span>
            </div>
          </div>
          <!--左侧部分结束-->
        </div>
        <div class="overview-right">
          <div class="delivery-score">
            <p class="score">{{ratings.delivery_score}}</p>
            <p class="text">配送评分</p>
          </div>
        </div>
      </div>
      <!--公共使用的间隔线-->
      <Split></Split>
      <!--间隔线下面的内容-->
      <div class="content">
        <!--tab切换部分开始-->
        <div class="rating-select" v-if="ratings.tab">
          <span class="item" :class="{'active':selectType==2}" @click="selectTypeFn(0)">
            {{ratings.tab[0].comment_score_title}}
          </span>
          <span class="item" :class="{'active':selectType==1}" @click="selectTypeFn(1)">
            {{ratings.tab[1].comment_score_title}}
          </span>
          <span class="item" :class="{'active':selectType==0}" @click="selectTypeFn(2)">

            <img v-show="selectType!=0" src="./img/icon_sub_tab_dp_normal@2x.png" />
            <img v-show="selectType==0" src="./img/icon_sub_tab_dp_highlighted@2x.png" />
            {{ratings.tab[2].comment_score_title}}
          </span>
        </div>
        <!--tab切换部分结束-->

        <!--label部分开始-->
        <div class="labels-view">
          <span class="item" :class="{'heigligh':item.label_star>0}" v-for="(item,index) in ratings.labels" :key="index">
            {{item.content}}{{item.label_count}}
          </span>
        </div>
        <!--label部分结束-->
        <!--评价列表部分开始-->
        <ul class="rating-list">
          <!--复制productDetail部分的评论-->
          <li v-for="(comment,index) in selectComments" :key="index" class="comment-item">
            <!--评论列表结构-->
            <div class="comment-header">
              <img :src="comment.user_pic_url" v-if="comment.user_pic_url" />
              <img src="./img/anonymity.png" v-if="!comment.user_pic_url" />
            </div>
            <div class="comment-main">
              <div class="user">
                {{comment.user_name}}
              </div>
              <div class="time">
                {{formatDate(comment.comment_time)}}
              </div>
              <!--添加一个评分-->
              <div class="star-wrapper">
                <span class="text">评分</span>
                <Star :score="comment.order_comment_score" class="star"></Star>
              </div>
              <!--添加评分结束-->
              <div class="content">
                {{comment.comment}}
              </div>
            </div>
          </li>
        </ul>
        <!--评价列表部分结束-->

      </div>
    </div>
  </div>
</template>

<script>

  import Bscroll from 'better-scroll'
  import Split from "../split/split.vue"
  import Star from "../../star/Star.vue"
  import axios from 'axios'
  const ALL = 2;
  const PICTURE = 1;
  const COMMENT = 0;
  export default {
    components: {
      Split,
      Star
    },
    data() {
      return {
        ratings: {},
        selectComments: [],
        selectType:2,
        total_Y:0
      }
    },
    methods: {
      selectTypeFn(type) {
        if (type == 2) {
          this.selectComments = this.ratings.comments_dp.comments;
          this.selectType=0
        } else if (type == 1) {
          let arr = [];
          this.selectType=1
          this.ratings.comments.forEach((res) => {
            if (res.comment_pics.length != 0) {
              arr.push(res);
            }
            this.selectComments = arr
          })
        } else {
          this.selectType=2
          this.selectComments = this.ratings.comments
        }
      },
      formatDate(time) {
        let date = new Date(time * 1000);
        let fmt = 'yyyy.MM.dd';
        if (/(y+)/.test(fmt)) { // 年
          let year = date.getFullYear().toString();
          fmt = fmt.replace(RegExp.$1, year);
        }
        if (/(M+)/.test(fmt)) { // 月
          let mouth = date.getMonth() + 1;
          if (mouth < 10) {
            mouth = '0' + mouth;
          }
          fmt = fmt.replace(RegExp.$1, mouth);
        }
        if (/(d+)/.test(fmt)) { // 日
          let mydate = date.getDate();
          if (mydate < 10) {
            mydate = '0' + mydate;
          }
          fmt = fmt.replace(RegExp.$1, mydate);
        }
        return fmt;
      }
    },
    created() {
      axios.get('/ratings').then((res) => {
        console.log(res.data);
        this.ratings = res.data.data;
        this.selectComments = this.ratings.comments;
        this.$nextTick(()=>{
          if(!this.ratingView){
          	this.ratingView=new Bscroll(this.$refs.ratingView,{
          		click:true
          	})
          }else{
          	//已经发生变化刷新
          	this.ratingView.refresh();
          }
          var rathing_s=(e)=>{
            console.log(res);
            let max_scroll=this.$refs.ratingView.scrollHeight;
            this.total_Y += e.deltaY;
            if (this.total_Y <= 0) {
              this.total_Y = 0;
            }
            else if (this.total_Y >= max_scroll) {
              this.total_Y = max_scroll;
            }
            this.ratingView.scrollTo(0, -this.total_Y);
          }
          this.$refs.ratingView.addEventListener("wheel", rathing_s,);////
        })
      })
    },
    computed: {

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 @import"./ratings.css"
</style>
