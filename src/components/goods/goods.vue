<template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuwrapper">
			<ul>
				<li v-for="(item,index) in goods" class="menu-item" @click="selectMenu(index, $event)" :class="{'current': currentIndex === index}">
					<span class="text">
						<span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>
						{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodswrapper">
			<ul>
				<li v-for="(item,index) in goods" class="food-list food-list-hook">
					<h1 class="food-title">{{item.name}}</h1>
					<ul>
						<li v-for="(food,index) in item.foods" class="food-item">
							<div class="icon">
								<img :src="food.icon" width="57" height="57" alt="">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span class="count">月售{{food.sellCount}}</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">￥{{food.price}}</span>
									<span v-if="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
								</div>
								<div class="cartcontrol-wrapper">
									<cartcontrol @cartadd="test($event)" :food="food"></cartcontrol>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
		<shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
	</div>
</template>
<script type="text/ecmascript-6">
	import BScroll from 'better-scroll'
	import shopcart from '@/components/shopcart/shopcart'
	import cartcontrol from '@/components/cartcontrol/cartcontrol'

	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data () {
			return {
				goods: [],
				listHeight: [],
				scrollY: 0
			}
		},
		computed: {
			currentIndex () {
				for (let i = 0; i < this.listHeight.length; i++) {
					let height1 = this.listHeight[i]
					let height2 = this.listHeight[i + 1]
					if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
						return i
					}
				}
				return 0
			},
			selectFoods () {
				let foods = []
				this.goods.forEach((good) => {
					good.foods.forEach((food) => {
						if (food.count > 0) {
							foods.push(food)
						}
					})
				})
				return foods
			}
		},
		created () {
			// this.$on('cart.add', (target) => {
			// 	this._drop(target)
			// })
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
			this.$http.get('/api/goods').then(response => {
				this.goods = response.data.data
				this.$nextTick(() => {
					this._initScroll()
					this._calculateHeight()
				})
				console.log(response.data.data)
			})
		},
		components: {
			shopcart,
			cartcontrol
		},
		methods: {
			test (target) {
				this._drop(target)
			},
			_initScroll () {
				this.menuScroll = new BScroll(this.$refs.menuwrapper, {
					click: true
				})
				this.foodsScroll = new BScroll(this.$refs.foodswrapper, {
					click: true,
					probeType: 3
				})
				this.foodsScroll.on('scroll', (pos) => {
					this.scrollY = Math.abs(Math.round(pos.y))
				})
			},
			_calculateHeight () {
				let foodList = this.$refs.foodswrapper.getElementsByClassName('food-list-hook')
				let height = 0
				this.listHeight.push(height)
				for (let item of foodList) {
					height += item.clientHeight
					this.listHeight.push(height)
				}
			},
			selectMenu (index, event) {
				let foodList = this.$refs.foodswrapper.getElementsByClassName('food-list-hook')
				let el = foodList[index]
				this.foodsScroll.scrollToElement(el, 300)
				if (!event.constructed) {
					return
				}
			},
			_drop (target) {
				this.$refs.shopcart.drop(target)
			}
		}
	}
</script>
<style>
	.goods {
		display: flex;
		overflow: hidden;
		position: absolute;
		top: 174px;
		bottom: 46px;
		width: 100%;
	}
	.goods .menu-wrapper {
		flex: 0 0 80px;
		width: 80px;
		background-color: #f3f5f7;
	}
	.goods .foods-wrapper {
		flex: 1;
	}
	.menu-wrapper .menu-item {
		display: table;
		width: 56px;
		height: 54px;
		padding: 0 12px;
		line-height: 14px;
	}
	.menu-wrapper .menu-item .icon {
		display: inline-block;
		width: 12px;
		height: 12px;
		margin-right: 2px;
		background-size: 12px;
		background-repeat: no-repeat;
		vertical-align: top;
	}
	.menu-wrapper .menu-item .text {
		display: table-cell; 
		width: 56px;
		font-size: 12px;
		vertical-align: middle;
		border-bottom: 0.5px solid rgba(7,17,27,0.1);
	}
	.menu-wrapper .current {
		position: relative;
		margin-top: -1px;
		z-index: 10;
		background-color: #fff;
		font-weight: 700;
	}
	.menu-wrapper .current .text {
		border: none;
	}
	.menu-wrapper .menu-item .decrease {
		background-image: url(decrease_3@2x.png);
	}
	.menu-wrapper .menu-item .discount {
		background-image: url(discount_3@2x.png);
	}
	.menu-wrapper .menu-item .guarantee {
		background-image: url(guarantee_3@2x.png);
	}
	.menu-wrapper .menu-item .invoice {
		background-image: url(invoice_3@2x.png);
	}
	.menu-wrapper .menu-item .special {
		background-image: url(special_3@2x.png);
	}
	.foods-wrapper .food-title {
		padding-left: 14px;
		height: 26px;
		line-height: 26px;
		border-left: 2px solid #d9dde1;
		font-size: 12px;
		color: rgb(147,153,159);
		background-color: #f3f5f7;
	}
	.foods-wrapper .food-item {
		position: relative;
		display: flex;
		margin: 18px;
		padding-bottom: 18px;
		border-bottom: 0.5px solid rgba(7,17,27,0.1);

	}
	.foods-wrapper .food-item:last-child {
		border: none;
		margin-bottom: 0;
	}
	.food-item .icon {
		flex: 0 0 57px;
		margin-right: 10px;
	}
	.food-item .content {
		flex: 1;
	}
	.food-item .content .name {
		margin: 2px 0 8px 0;
		height: 14px;
		line-height: 14px;
		font-size: 14px;
		color: rgb(7,17,27);
	}
	.food-item .content .desc {
		margin-bottom: 8px;
		line-height: 12px;
		font-size: 10px;
		color: rgb(147,153,159);
	}
	.food-item .content .extra {
		line-height: 10px;
		font-size: 10px;
		color: rgb(147,153,159);
	}
	.food-item .content .extra .count {
		margin-right: 12px;
	}
	.price {
		font-weight: 700;
		line-height: 24px;
	}
	.price .now {
		margin-right: 18px;
		font-size: 14px;
		color: rgb(240,20,20);
	}
	.price .old {
		text-decoration: line-through;
		font-size: 10px;
		color: rgb(147,153,159);
	}
	.cartcontrol-wrapper {
		position: absolute;
		right: 0;
		bottom: 12px;
	}
</style>
