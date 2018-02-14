<template>
  <div class="header">
    <div class="content-wrapper">
    	<div class="avatar">
    		<img width="64" height="64" :src="seller.avatar">
    	</div>
    	<div class="content">
    		<div class="title">
    			<span class="brand"></span>
    			<span class="name">{{seller.name}}</span>	
    		</div>
    		<div class="description">
    			{{seller.description}}/{{seller.deliveryTime}}分钟送达
    		</div>
    		<div v-if="seller.supports" class="support">
    			<span class="icon" :class="classMap[seller.supports[0].type]"></span>
    			<span class="text">{{seller.supports[0].description}}</span>
    		</div>
    	</div>
    	<div v-if="seller.supports" class="support-count">
    		<span class="count">{{seller.supports.length}}个</span>
    		<i class="icon-arrow-right">></i>
    	</div>
    </div>
    <div class="bulletin-wrapper">
    	<span class="bulletin-title"></span>
    	<span class="bulletin-text">{{seller.bulletin}}</span>
    	<i class="bulletin-icon-arrow-right">></i>
    </div>
    <div class="background">
    	<img :src="seller.avatar" width="100%" height="100%">
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default{
  	props:["seller"],
  	created(){
  		this.classMap = ['decrease','discount','special','invoice','guarantee'];
  	}
  }
</script>

<style>
	.header{
		color: #fff;
		position: relative;
		background: rgba(7,17,27,0.5);
	}
	.content-wrapper{
		padding: 24px 12px 18px 24px;
		font-size: 0;
		position: relative;
	}
	.avatar,.content{
		display: inline-block;
		margin-left: 16px;
		vertical-align: top;
	}
	.avatar img{
		border-radius: 2px;
	}
	.title{
		margin: 2px 0 8px 0;
	}
	.brand{
		width: 30px;
		height: 18px;
		display: inline-block;
		background: url('./brand@2x.png');
		background-size: 100% 100%;
		background-repeat: no-repeat;
	}
	.name{
		margin-left: 6px;
		font-size: 16px;
		line-height: 18px;
		font-weight: bold;
	}
	.description{
		margin-bottom: 10px;
		line-height: 12px;
		font-size: 12px;
	}
	.icon{
		display: inline-block;
		width: 12px;
		height: 12px;
		margin-right: 4px;
		background-size: 100% 100%;
		background-repeat: no-repeat;
		vertical-align: top;
	}
	.decrease{
		background: url('./decrease_1@2x.png');
	}
	.discount{
		background: url('./discount_1@2x.png');
	}
	.special{
		background: url('./special_1@2x.png');
	}
	.invoice{
		background: url('./invoice_1@2x.png');
	}
	.guarantee{
		background: url('./guarantee_1@2x.png');
	}
	.text{
		font-size: 12px;
		line-height: 12px;
	}
	.support-count{
		position: absolute;
		right: 12px;
		bottom: 14px;
		padding: 0 8px;
		height: 24px;
		line-height: 24px;
		border-radius: 14px;
		background: rgba(0,0,0,0.2);
		text-align: center;
	}
	.count{
		font-size: 10px;
		vertical-align: top;
	}
	.icon-arrow-right{
		font-size: 10px;
		margin-left: 5px;
		line-height: 24px;
	}
	.bulletin-wrapper{
		height: 28px;
		line-height: 28px;
		padding: 0 22px 0 12px;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		position: relative;
		background: rgba(7,17,27,0.2);
	}
	.bulletin-title{
		display: inline-block;
		width: 22px;
		height: 12px;
		background: url('./bulletin@2x.png');
		background-size: 100% 100%;
		background-repeat: no-repeat;
		vertical-align: top;
		margin-top: 7px;
	}
	.bulletin-text{
		font-size: 10px;
		vertical-align: top;
		margin: 0 4px;
	}
	.bulletin-icon-arrow-right{
		position: absolute;
		font-size: 10px;
		right: 12px;
	}
	.background{
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
		filter: blur(10px);
	}
</style>