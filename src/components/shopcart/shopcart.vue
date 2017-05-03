<template>
	<div class="shopcart">
		<div class="content" @click="toggleList">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'highlight': totalCount>0}">
						<i class="icon-shopping_cart" :class="{'highlight': totalCount>0}"></i>
					</div>
					<div class="num" v-show="totalCount>0">{{totalCount}}</div>
				</div>
				<div class="price" :class="{'highlight': totalCount>0}">￥{{totalPrice}}</div>
				<div class="desc">另需配送费{{deliveryPrice}}元</div>
			</div>
			<div class="content-right">
				<div class="pay" :class="payClass">
					{{payDesc}}
				</div>

			</div>
		</div>
		<div class="ball-container">
			<transition-group tag="div" name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
				<div v-for="(ball, index) in balls" v-show="ball.show" class="ball" :key="index">
					<div class="inner inner-hook"></div>
				</div>
			</transition-group>
		</div>
		<transition name="fold">
			<div class="shopcart-list" v-show="listShow">
				<div class="list-header">
					<h1 class="title">购物车</h1>
					<span class="empty" @click="empty">清空</span>
				</div>
				<div class="list-content" ref="listcontent">
					<ul>
						<li class="food" v-for="food in selectFoods">
							<span class="name">{{food.name}}</span>
							<div class="price">
								<span>￥{{food.price*food.count}}</span>
							</div>
							<div class="cartcontrol-list-wrapper">
								<cartcontrol :food="food"></cartcontrol>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</transition>
		<transition name="fade">
			<div class="list-mask" v-show="listShow"></div>
		</transition>
	</div>
</template>
<script type="text/ecmascript-6">
	import cartcontrol from '@/components/cartcontrol/cartcontrol'
	import BScroll from 'better-scroll'

	export default {
		components: {
			cartcontrol
		},
		data () {
			return {
				balls: [
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					},
					{
						show: false
					}
				],
				dropBalls: [],
				fold: true
			}
		},
		props: {
			deliveryPrice: {
				type: Number
			},
			minPrice: {
				type: Number
			},
			selectFoods: {
				type: Array,
				default () {
					return []
				}
			}
		},
		computed: {
			totalPrice () {
				let total = 0
				this.selectFoods.forEach((food) => {
					total += food.price * food.count
				})
				return total
			},
			totalCount () {
				let count = 0
				this.selectFoods.forEach((food) => {
					count += food.count
				})
				return count
			},
			payDesc () {
				if (this.totalPrice === 0) {
					return `￥${this.minPrice}元起送`
				} else if (this.totalPrice < this.minPrice) {
					let diff = this.minPrice - this.totalPrice
					return `还差￥${diff}元起送`
				} else {
					return '去结算'
				}
			},
			payClass () {
				if (this.totalPrice >= this.minPrice) {
					return 'enough'
				} else {
					return 'not-enough'
				}
			},
			listShow () {
				if (!this.totalCount) {
					this.fold = true
					return false
				}
				let show = !this.fold
				if (show) {
					this.$nextTick(() => {
						if (!this.scroll) {
							this.scroll = new BScroll(this.$refs.listcontent, {
								click: true
							})
						} else {
							this.scroll.refresh()
						}
					})
				}
				return show
			}
		},
		methods: {
			drop (el) {
				for (let i = 0; i < this.balls.length; i++) {
					let ball = this.balls[i]
					if (!ball.show) {
						ball.show = true
						ball.el = el
						this.dropBalls.push(ball)
						return
					}
				}
			},
			beforeEnter (el) {
				let count = this.balls.length
				while (count--) {
					let ball = this.balls[count]
					if (ball.show) {
						let rect = ball.el.getBoundingClientRect()
						let x = rect.left - 32
						let y = -(window.innerHeight - rect.top - 22)
						el.style.display = ''
						el.style.webKitTransform = `translate3d(0,${y}px,0)`
						el.style.transform = `translate3d(0,${y}px,0)`
						let inner = el.getElementsByClassName('inner-hook')[0]
						inner.style.webKitTransform = `translate3d(${x}px,0,0)`
						inner.style.transform = `translate3d(${x}px,0,0)`
					}
				}
			},
			empty () {
				this.selectFoods.forEach((food) => {
					food.count = 0
				})
			},
			enter (el) {
				/* eslint-disable no-unused-vars */
				let rf = el.offsetHeight
				this.$nextTick(() => {
					el.style.display = ''
					el.style.webKitTransform = 'translate3d(0,0,0)'
					el.style.transform = 'translate3d(0,0,0)'
					let inner = el.getElementsByClassName('inner-hook')[0]
					inner.style.webKitTransform = 'translate3d(0,0,0)'
					inner.style.transform = 'translate3d(0,0,0)'
				})
			},
			afterEnter (el) {
				let ball = this.dropBalls.shift()
				if (ball > 0) {
					ball.show = false
					el.style.display = 'none'
				}
			},
			toggleList () {
				if (!this.totalCount) {
					return
				}
				this.fold = !this.fold
			}
		}
	}

</script>
<style scoped>
	.shopcart {
		position: fixed;
		bottom: 0;
		left: 0;
		z-index: 50;
		width: 100%;
		height: 48px;
		background-color: #999;
	}
	.shopcart .content {
		display: flex;
		background-color: #141d27;
		font-size: 0;
		z-index: 50;
	}
	.content .content-left {
		flex: 1;
	}
	.content-left .logo-wrapper {
		display: inline-block;
		position: relative;
		top: -10px;
		margin: 0 12px;
		padding: 6px;
		width: 56px;
		height: 56px;
		z-index: 51;
		box-sizing: border-box;
		vertical-align: top;
		border-radius: 50%;
		background-color: #141d27;
	}
	.logo-wrapper .logo {
		width: 100%;
		height: 100%;
		border-radius: 50%;
		background-color: #2b343c;
		text-align: center;
	}
	.logo-wrapper .highlight {
		background-color: rgb(0,160,220);
		color: #fff;
	}
	.logo-wrapper .num {
		position: absolute;
		top: 0;
		right: 0;
		width: 24px;
		height: 16px;
		line-height: 16px;
		text-align: center;
		border-radius: 16px;
		font-size: 9px;
		color: #fff;
		background-color: rgb(240,20,20);
		box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
	}
	.logo .icon-shopping_cart {
		font-size: 24px;
		color: #80858a;
		line-height: 44px;
	}
	.logo .highlight {
		color: #fff;
	}
	.content-left .price {
		display: inline-block;
		vertical-align: top;
		margin-top: 12px;
		line-height: 24px;
		color: rgba(255,255,255,0.4);
		margin-top: 12px;
		box-sizing: border-box;
		padding-right: 12px;
		border-right: 1px solid rgba(255,255,255,0.1);
		font-size: 16px;
		font-weight: 700;
	}
	.highlight {
		color: #fff !important;
	}
	.content-left .desc {
		display: inline-block;
		vertical-align: top;
		line-height: 24px;
		margin: 12px 0 0 12px;
		font-size: 10px;
		color: rgba(255,255,255,0.4)
	}
	.content .content-right {
		width: 105px;
		flex: 0 0 105px;
		background-color: #2b343c;
	}
	.content-right .pay {
		height: 48px;
		line-height: 48px;
		text-align: center;
		font-size: 12px;
		color: rgba(255,255,255,0.4);
		font-weight: 700;
	}
	.content-right .not-enough {
		background-color: #2b333b;
	}
	.content-right .enough {
		background-color: #00b43c;
		color: #fff;
	}
	.ball-container {

	}
	.ball-container .ball {
		position: fixed;
		left: 32px;
		bottom: 22px;
		z-index: 200;
	}
	.drop-enter-active {
		transition: all 0.4s cubic-bezier(0.49,-0.29,0.75,0.41);
	}
	.drop-enter {

	}
	.drop-enter-active 	.ball  {
		transition: all 0.4s;
	}
	/*	.drop-enter-active 	*/
	.drop-enter-active .inner {
		width: 16px;
		height: 16px;
		border-radius: 50%;
		background-color: rgb(0,160,220);
		transition: all 0.4s linear;
	}
	.shopcart-list {
		position: absolute;
		top: 0;
		left: 0;
		z-index: 50;
		width: 100%;
		transform: translate3d(0,-100%,0);
	}
	.shopcart-list .list-header {
		height: 40px;
		line-height: 40px;
		padding: 0 18px;
		background-color: #f3f5f7;
		border-bottom: 1px solid rgba(7,17,27,0.1)
	}
	.list-header .title {
		float: left;
		font-size: 14px;
		color: rgb(7,17,27);
	}
	.list-header .empty {
		float: right;
		font-size: 12px;
		color: rgb(0,160,220);
	}
	.shopcart-list .list-content {
		padding: 0 18px;
		max-height: 217px;
		background-color: #fff;
		overflow: hidden;
	}
	.list-content .food {
		position: relative;
		padding: 12px 0;
		box-sizing: border-box;
		border-bottom: 0.5px solid rgba(7,17,27,0.1);
	}
	.list-content .name {
		line-height: 24px;
		font-size: 14px;
		color: rgb(7,17,27);
	}
	.list-content .price {
		position: absolute;
		right: 90px;
		bottom: 12px;
		line-height: 24px;
		font-size: 14px;
		color: rgb(240,20,20);
	}
	.cartcontrol-list-wrapper {
		position: absolute;
		right: 0;
		bottom: 6px;

	}
	.list-mask {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
		background-color: rgba(7,17,27,0.6);
		backdrop-filter: blur(10px);
	}
	.fold-enter-active {
		transition: all 0.5s linear;
		transform: translate3d(0,-100%,0);
	}
	.fold-enter {
		transform: translate3d(0,0,0);
	}
	.fold-leave-active {
		transition: all 0.5s linear;
		transform: translate3d(0,0,0);
	}
	.fold-leave {
		transform: translate3d(0,-100%,0);
	}
	.fade-enter-active {
		transition: all 0.5s linear;
		opacity: 1;
	}
	.fade-enter {
		opacity: 0;
	}
	.fade-leave-active {
		transition: all 0.5s linear;
	}
</style>
