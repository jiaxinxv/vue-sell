<template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li class="menu-item" v-for="(item,index) in goods" :class="{'current':currentIndex === index}" @click="selectMenu(index)">
					<span class="text">
						<span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul>
				<li v-for="item in goods" class="food-list food-list-hook">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li v-for="food in item.foods" class="food-item">
							<div class="icon">
								<img width="57" height="57" :src="food.icon">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span>月售{{food.sellCount}}份</span>
									<span>好评率{{food.rating}}</span>
								</div>
								<div class="price">
									<span class="now">￥{{food.price}}</span>
									<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
	</div>
</template>

<script type="text/ecmascript-6">
	import BScroll from 'better-scroll';
	const ERR_OK = 0;
	export default{
		props:['seller'],
		data(){
			return{
				goods:[],
				listHeight:[],
				scrollY:0
			}
		},
		created(){
			this.classMap = ['decrease','discount','special','invoice','guarantee'];
			this.$http.get('/api/goods').then((response) => {
				if(response.data.errno === ERR_OK){
					this.goods = response.data.data;
					this.$nextTick(() => {
						this._initScroll();
						this._calculateHeight();
					});
					
				}
			});
		},
		methods:{
			_initScroll(){
				this.menuScroll = new BScroll(this.$refs.menuWrapper,{
					click:true
				});
				this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
					probeType:3
				});
				this.foodsScroll.on('scroll',(pos) => {
					this.scrollY = Math.abs(Math.round(pos.y));
				})
			},
			_calculateHeight(){
				let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
				let height = 0;
				this.listHeight.push(height);
				for(let i = 0; i < foodList.length; i++){
					let item = foodList[i];
					height += item.clientHeight;
					this.listHeight.push(height);					
				}
			},
			selectMenu(index){
				let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
				let el = foodList[index];
				this.foodsScroll.scrollToElement(el,300);
			}
		},
		computed:{
			currentIndex(){
				for(let i = 0; i < this.listHeight.length; i++){
					let height1 = this.listHeight[i];
					let height2 = this.listHeight[i+1];
					if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
						return i;
					}
				}
				return 0;
			}
		}
	}
</script>

<style lang="less" rel="stylesheet/less">
	.goods{
		display: flex;
		overflow: hidden;
		position: absolute;
		top: 181px;
		bottom: 46px;
		width: 100%;
		.menu-wrapper{
			flex: 0 0 80px;
			width: 80px;
			background: #f3f5f7;
			.menu-item{
				display: table; 
				width: 80px !important;
				height: 54px;
				width: 56px;
				line-height: 14px;
				padding: 0 4px;
		
				&.current{
					width: 80px;
					position: relative;
					z-index: 10;
					background: #fff;
					font-weight: 700;
				}
				.icon{
					display: inline-block;
					width: 12px;
					height: 12px;
					margin-right: 2px;
					background-size: 100% 100%;
					background-repeat: no-repeat;
					vertical-align: top;
					&.decrease{
						background-image: url('./decrease_3@2x.png');
					}
					&.discount{
						background-image: url('./discount_3@2x.png');
					}
					&.special{
						background-image: url('./special_3@2x.png');
					}
					&.invoice{
						background-image: url('./invoice_3@2x.png');
					}
					&.guarantee{
						background-image: url('./guarantee_3@2x.png');
					}
				}
				.text{
					display: table-cell;
					width: 56px;
					vertical-align: middle;
					font-size: 12px;
					border-bottom: 1px solid rbga(7,17,27,0.1);
				}
			}
		}
		.foods-wrapper{
			flex: 1;
			margin-top: -2px;
			.title{
				padding: 0 14px;
				height: 26px;
				line-height: 26px;
				border-left: 2px solid #d9dde1;
				font-size: 12px;
				coloe: rgb(147,153,159); 
				background: #f3f5f7;
			}
			.food-item{
				display: flex;
				margin: 18px;
				padding-bottom: 18px;
				border-bottom: 1px solid rbga(7,17,27,0.1);
				&:last-child{
					border-bottom: 0px solid;
					margin-bottom: 0;
				}
				.icon{
					flex: 0 0 57px;
					margin-right: 10px;
				}
				.content{
					flex: 1;
					.name{
						margin: 2px 0 8px 0;
						height: 14px;
						line-height: 14px;
						font-size: 14px;
						color: rbg(7,17,27);
					}
					.desc,.extra{
						line-height: 10px;
						font-size: 10px;
						color: rgb(147,153,159); 
					}
					.desc{
						margin-bottom: 8px;
						line-height: 12px;
					}
					.extra{
						&.count{
							margin-right: 12px;
						}
					}
					.price{
						font-weight: 700;
						line-height: 24px;
						.now{
							margin-right: 8px;
							font-size: 14px;
							color: rgb(240,20,20);
						}
						.old{
							text-decoration: line-through;
							font-size: 10px;
							color: rgb(147,153,159); 
						}
					}
				}
			}
		}
	}
</style>
