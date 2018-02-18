<template>
	<div class="cartcontrol">
		<!-- <transition name="move"> -->
			<div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">-</div>
		<!-- </transition>	 -->
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add" @click="addCart">+</div>
	</div>	
</template>

<script type="text/ecmascript-6">
	import Vue from 'vue';
	export default{
		props:{
			food:{
				type:Object
			}
		},
		// data(){
		// 	return{

		// 	}
		// },
		created(){
			
		},
		computed:{

		},
		methods:{
			addCart(){
				if(!this.food.count){
					Vue.set(this.food,'count',1);
				}else{
					this.food.count++;
				}
			},
			decreaseCart(){
				if(this.food.count){
					this.food.count--;
				}
			}
		}
	}
</script>

<style lang="less" rel="stylesheet/less">
	.cartcontrol{
		font-size: 0;
		.cart-decrease,.cart-add{
			display: inline-block;
			padding: 6px;
			line-height: 24px;
			font-size: 24px;
			color: rgb(0,160,220);
		}
		.cart-count{
			display: inline-block;	
			vertial-aligin: top;
			width: 12px;
			padding-top: 6px;
			line-height: 24px;
			text-align: center;
			font-size: 10px;
			color: rgb(147,153,159);
		}
	}
</style>
