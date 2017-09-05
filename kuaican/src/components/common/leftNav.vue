<template>
  <div class="left-nav">
    <ul>
    	<li>
    		<svg class="icon" aria-hidden="true">
		   		<use xlink:href="#icon-home"></use>	   		
			</svg>
			<b>主页</b>
    	</li>
    	<li>
    		<svg class="icon" aria-hidden="true">
		   		<use xlink:href="#icon-birthdaycake"></use>   		
			</svg>
			<b>商品</b>
    	</li>
    	<li>
    		<svg class="icon" aria-hidden="true">
		   		<use xlink:href="#icon-card"></use>	
			</svg>
			<b>会员</b>
    	</li>
    	<li>
    		<svg class="icon" aria-hidden="true">
		   		<use xlink:href="#icon-star"></use>		   		
			</svg>
			<b>收藏</b>
    	</li>
    </ul>
  </div>
</template>
 
<script>
export default {
  name: 'leftNav',
  data () {
    return {
    }
  }
}
</script>
<style>
 .left-nav{
       color:#fff;
       /*font-size:10px;*/
       height:100%;
       background-color: #1D8ce0;
       float:left;
       width:5%;
    }
    .icon {
       width: 30px;
       height: 30px;
       /*vertical-align: -0.15em;
       fill: currentColor;
       overflow: hidden;*/
    }
    b{
    	display: block;
    }
    .left-nav ul{
        padding:0px;
        margin: 0px;
    }
    .left-nav li{
        list-style: none;
        text-align: center;
        border-bottom:1px solid #20a0ff;
        padding:10px;
    }
</style>