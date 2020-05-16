<template>
    <div class="hello">
      <div class="main-wrapper" ref="wrapper">
      		<ul>
				<li>1</li>
				<li>2</li>
				<li>3</li>
				<li>4</li>
				<li>5</li>
				<li>6</li>
				<li>7</li>
				<li>8</li>
				<li>9</li>
				<li>10</li>
				<li>11</li>
				<li>12</li>
				<li>13</li>
				<li>14</li>
				<li>15</li>
				<li>16</li>
				<li>17</li>
				<li>18</li>
				<li>19</li>
				<li>20</li>
      		</ul>
      </div>
    </div>
</template>

<script>
import Bscroll from 'better-scroll'
export default {
  name: 'bscroll',
  data () {
    return {
     
    }
  },
  methods:{
  	initScroll() {
  		this.scroll = new Bscroll(this.$refs.wrapper, {
        	click:true
        })
        console.log(this.scroll)
  	}
  },
  mounted() {
    this.$nextTick(() => {
        this.initScroll()
      })
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-wrapper {
	position: absolute;
	top: 0;
	right: 0;
	left: 0;
	bottom: 0;
	width: 100%;
	height: 100%;
	overflow: hidden;
}
ul li {
	height: 60px;
	line-height: 60px;
	background-color: #7e7e7e;
	border-bottom: 2px solid red;
	overflow: hidden;
}
</style>
