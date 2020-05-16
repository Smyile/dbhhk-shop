<template>
  <div class="star">
  	<span class="star-item" v-for="(itemClass,index) in itemClasses" :key="index" :class="itemClass"></span>
  </div>
</template>

<script>
	//星星长度
	const LENGTH=5
	//星星状态
	const CLS_ON='on'//全星
	const CLS_HALF="half" //半星
	const CLS_OFF='off' //补齐
  export default{
    props:{
    	score:{
    		type:Number
    	}
    },
    computed:{
    	itemClasses(){
    		//定义一个空数组,存放星星状态
    		let result=[]
    		//4.7,对数据处理4.7*2向下取整再除以2就是4.5,有半星
    		let score=Math.floor(this.score*2)/2
    		//半星,对1取余如果!==0说明有小数,说明半星
    		let hasDecimal=score%1!==0
    		//全星
    		let integer=Math.floor(score)
    		//遍历全星,看有几个星星
    		for(let i=0;i<integer;i++){
    			result.push(CLS_ON);
    		}
    		//处理半星,半星只有一个,判断就行,有的话push进去
    		if(hasDecimal){
    			result.push(CLS_HALF);
    		}
    		//补齐星星
    		while(result.length<length){
    			result.push(CLS_OFF);
    		}
    		return result;
    	}
    }
  }   
</script>

<style>
.star{
		font-size: 0;
	}
	.star .star-item{
		display: inline-block;
		width: 10px;
		height: 10px;
		margin-right: 3px;
		background-repeat: no-repeat;
		background-size: 10px 10px;
	}
	.star .star-item:last-child{
		margin-right: 0;
	}
	
	/* 三种图片类型*/
	.star .on{
		background-image: url(./img/star24_on@2x.png);
	}
	.star .half{
		background-image: url(./img/star24_half@2x.png);
	}
	.star .off{
		background-image: url(./img/star24_off@2x.png);
	}
</style>

