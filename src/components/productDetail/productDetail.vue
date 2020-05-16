<template>
	<transition name="food-detail">
	<div class="food" v-show="showFlag" ref="foodView">
		<div class="food-wrapper">
			<!--内容部分-->
			<div class="food-content">
				<div class="img-wrapper">
					<!--一个大图,从数据中拿-->
					<img class="food-img" :src="food.picture"/>
					<!--三个小图-->
					<!--关闭-->
					<span class="fa fa-close close-bt"
						@click="showView"
						></span>
					<!--分享-->
					<img class="share-bt" src="./img/share.png" />
					<!--更多-->
					<img class="more-bt" src="./img/more.png" />
				</div>

				<!--文字部分-->
				<div class="content-wrapper">
					<!--名字-->
					<h3 class="name">{{food.name}}</h3>
					<!--销售了多少-->
					<p class="saled">{{food.month_saled_content}}</p>
					<!--图片-->
					<img class="product" v-show="food.product_label_picture" :src="food.product_label_picture" />
					<p class="price">
						<!--多少钱-->
						<span class="text">￥{{food.min_price}}</span>
						<!--单位-->
						<span class="unit">/{{food.unit}}</span>
					</p>
					<!--选规格,如果已经点了选规格变成加减号,所以加入加减号组件-->
					<div class="cartcontrol-wrapper">
						<CartControl :food="food"></CartControl>
					</div>
					<!--如果没有点选规格则是黄色选规格文字-->
					<div class="buy"
						@click="addProduct"
						 v-show="!food.count||food.count==0">
						选规格
					</div>
				</div>

			</div>
			<Split></Split>

			<!--外卖评价-->
			<div class="rating-wrapper">
				<!--评价头部-->
				<div class="rating-title">
					<div class="like-ratio" v-if="food.rating">
						<span class="title">{{food.rating.title}}</span>
						<span class="retio">
							(
							{{food.rating.like_ratio_desc}}
							<i>{{food.rating.like_ratio}}</i>
							)
						</span>
					</div>
					<div class="snd-title" v-if="food.rating">
						<span class="text">{{food.rating.snd_title}}</span>
						<span class="fa fa-angle-right"></span>
					</div>
				</div>

				<ul class="rating-content" v-if="food.rating">
					<li v-for="(comment,index) in food.rating.comment_list"
						:key="index"
						class="comment-item">
						<!--评论列表结构-->
						<div class="comment-header">
							<img :src="comment.user_icon" v-if="comment.user_icon" />
							<img src="./img/anonymity.png" v-if="!comment.user_icon" />
						</div>
						<div class="comment-main">
							<div class="user">
								{{comment.user_name}}
							</div>
							<div class="time">
								{{comment.comment_time}}
							</div>
							<div class="content">
								{{comment.comment_content}}
							</div>
						</div>
					</li>
				</ul>
			</div>

		</div>
	</div>
	</transition>
</template>

<script>
	import Vue from 'vue'
	import BScroll from 'better-scroll'
	import CartControl from "../cartControl/cartControl"
	import Split from "../split/split"
	export default{
		components:{
			CartControl,
			Split
		},
		data(){
			return {
				showFlag:false
			}
		},
		props:{
			food:{
				type:Object
			}
		},
		methods:{
			showView(){
				this.showFlag=!this.showFlag
				this.$nextTick(()=>{
					if(!this.scroll){
						this.scroll=new BScroll(this.$refs.foodView,{
							click:true
						});
					}else{
						this.scroll.refresh()
					}
				})
			},
			addProduct(){
				//不能直接用this.count=0,因为count此时还不存在
				//需要引入vue用vue的set设置count
				Vue.set(this.food,'count',1);
				// Vue.set(this.food,'count',1);
			}
		}
	}
</script>

<style>
  @import url("./proDe.css");
</style>
<style src="../../common/css/font-awesome.css"></style>
