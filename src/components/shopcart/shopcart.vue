<template>
	<div>
		<div class="shopcart">
			<div class="content" @click="toggleList">
				<div class="content-left">
					<div class="logo-wrapper">
						<div class="logo" :class="{'highlight':totalCount>0}">
							<i class="icon-shopping-cart" :class="{'highlight':totalCount>0}">che</i>
						</div>	
						<div class="num" v-show="totalCount>0">{{totalCount}}</div>	
					</div>
					<div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
					<div class="desc">另需配送费{{deliveryPrice}}元</div>
				</div>
				<div class="content-right" @click.stop.prevent="pay">
					<div class="pay" :class="payClass">{{payDesc}}</div>
				</div>
			</div>
			<div class="shopcart-list" v-show="listShow">
				<div class="list-header">
					<h1 class="title">购物车</h1>
					<span class="empty" @click="empty">清空</span>
				</div>
				<div class="list-content" ref="listContent">
					<ul>
						<li class="food" v-for="food in selectFoods">
							<span class="name">{{food.name}}</span>
							<div class="price">
								<span class="">￥{{food.price*food.count}}</span>
							</div>
							<div class="cartcontrol-wrapper">
								<cartcontrol :food="food"></cartcontrol>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</div>	
		<transition name="fade">
			<div class="list-mask" v-show="listShow" @click="hideList"></div>
		</transition>	
	</div>
</template>

<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	import cartcontrol from './../cartcontrol/cartcontrol';
	export default{
		props:{
			deliveryPrice:{
				type:Number,
				default:0
			},
			minPrice:{
				type:Number,
				default:0
			},
			selectFoods:{
				type:Array,
				default(){
					return [];
				}
			}
		},
		data(){
			return{
				fold:true
			}
		},
		computed:{
			totalPrice(){
				let total = 0;
				this.selectFoods.forEach((food) => {
					total += food.price * food.count;
				});
				return total;
			},
			totalCount(){
				let count = 0;
				this.selectFoods.forEach((food) => {
					count += food.count;
				});
				return count;
			},
			payDesc(){
				if(this.totalPrice === 0){
					return `￥${this.minPrice}元起送`;
				}else if(this.totalPrice < this.minPrice){
					let diff = this.minPrice - this.totalPrice;
					return `还差￥${diff}元起送`;
				}else{
					return '去结算';
				}
			},
			payClass(){
				if(this.totalPrice < this.minPrice){
					return 'not-enouth';
				}else{
					return 'enough';
				}
			},
			listShow(){
				if(!this.totalCount){
					this.fold = true;
					return false;
				}
				let show = !this.fold;
				if(show){
					this.$nextTick(() => {
						if(!this.scroll){
							this.scroll = new BScroll(this.$refs.listContent,{
								click:true
							});
						}else{
							this.scroll.refresh();
						}						
					});
				}
				return show;
			}		
		},
		methods:{
			toggleList(){
				if(!this.totalCount){
					return;
				}
				this.fold = !this.fold;
			},
			empty(){
				this.selectFoods.forEach((food) => {
					food.count = 0;
				})
			},
			hideList(){
				this.fold = true;
			},
			pay(){
				if(this.totalPrice < this.minPrice){
					return;
				}
				window.alert(`支付${this.totalPrice}元`);
			}
		},
		components:{
			cartcontrol
		}

	}
</script>

<style lang="less" rel="stylesheet/less">
	.shopcart{
		position: fixed;
		left: 0;
		bottom: 0;
		z-index: 50;
		width: 100%;
		height: 48px;
		.content{
			display: flex;
			margin-left: 0;
			background: #141d27;
			font-size: 0;
			.content-left{
				flex: 1;
				.logo-wrapper{
					display: inline-block;
					position: relative;
					top: -10px;
					margin: 0 12px;
					padding: 6px;
					width: 56px;
					height: 56px;
					box-sizing: border-box;
					vertical-align: top;
					border-radius: 50%;
					background: #141d27;
					.logo{
						width: 100%;
						height: 100%;
						border-radius: 50%;
						background: #2b343c;
						text-align: center;
						&.highlight{
							background: rgb(0,160,220);
						}
						.icon-shopping-cart{
							font-size: 24px;
							color: #8085a;
							line-height: 44px;
							&.highlight{
								color: #fff;
							}
						}
					}	
					.num{
						position: absolute;
						top: 0;
						right: 0;
						width: 24px;
						height: 16px;
						line-height: 16px;
						text-align: center;
						border-radius: 16px;
						font-size: 9px;
						font-weight: 700;
						color: #fff;
						background: rgb(240,20,20);
						box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
					}				
				}
				.price{
					display: inline-block;
					vertical-align: top;
					line-height: 24px;
					margin-top: 12px;
					padding-right: 12px;
					box-sizing: border-box;
					border-right: 1px solid rgba(255,255,255,0.1);
					font-size: 16px;
					font-weight: 700;
					color: rgba(255,255,255,0.4);
					&.highlight{
						color: #fff;
					}
				}
				.desc{
					display: inline-block;
					vertical-align: top;
					line-height: 24px;
					margin: 12px 0 0 12px;
					color: rgba(255,255,255,0.4);
					font-size: 10px;
				}
			}
			.content-right{
				flex: 0 0 105px;
				width: 105px;
				background: #2b333b;
				.pay{
					height: 48px;
					line-height: 48px;
					text-align: center;
					font-size: 12px;
					color: rgba(255,255,255,0.4);
					font-weight: 700;
					&.not-enouth{
						background: #2b333b;
					}
					&.enough{
						background: #00b43c;
						color: #fff;
					}
				}
			}
		}
		.shopcart-list{
			position: absolute;
			bottom: 48px;
			left: 0;
			z-index: -1;
			width: 100%;
			.list-header{
				height: 40px;
				line-height: 40px;
				padding: 0 18px;
				background: #f3f5f7;
				border-bottom: 2px solid rbga(7,17,27,0.1);
				.title{
					float: left;
					font-size: 14px;
					color: rgb(7,17,27);
					margin-bottom: -2px;
				}
				.empty{
					float: right;
					font-size: 12px;
					color: rgb(0,160,220);
				}
			}
			.list-content{
				padding: 0 18px;
				width: 100%;
				max-height: 217px;
				background: #fff;
				overflow: hidden;
				.food{
					position: relative;
					padding: 12px 0;
					box-sizing: border-box;
					border-bottom: 1px solid rgba(7,17,27,0.1);
					.name{
						line-height: 24px;
						font-size: 14px;
						color: rgb(7,17,27);
					}
					.price{
						position: absolute;
						right: 90px;
						bottom: 12px;
						line-height: 24px;
						font-size: 14px;
						font-weight: 700;
						color: rgb(240,20,20);
					}
					.cartcontrol-wrapper{
						position: absolute;
						bottom: 6px;
						right: 0;
						width: 80px;
					}
				}
			}	
		}
	}
	.list-mask{
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 40;
		background: rgba(7,17,27,0.6);
		.fade-transition{
			opacity: 1;			
		}
		.fade-enter,&.enter-leave{
			opacity: 0;
			background: rgba(7,17,27,0); 
		}
	}
</style>
