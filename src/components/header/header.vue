<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img width="64" height="64" :src="seller.avatar" alt="">
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="desc">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<div v-if="seller.supports" class="supports">
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
			</div>
			<!-- content end -->
			<div v-if="seller.supports" class="support-count" @click="showDetail">
				<span class="count">{{seller.supports.length}}个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<!-- 下面是公告区 -->
		<div class="bulletin-wrapper" @click="showDetail">
			<span class="bulletin-title"></span>
			<span class="bulletin-text">{{seller.bulletin}}</span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
		<!-- 下面是背景图片 -->
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%" alt="">
		</div>
		<!-- 下面是弹层 -->
		<transition name="fade">
			<div v-show="detailShow" class="detail">
				<div class="detail-wrapper clearfix">
					<div class="detail main">
						<h1 class="detail-name">{{seller.name}}</h1>
						<div class="star-wrapper">
							<star :size="48" :score="seller.score"></star>
						</div>
						<div class="detail-title">
							<div class="line"></div>
							<div class="detail-text">优惠信息</div>
							<div class="line"></div>
						</div>
						<ul v-if="seller.supports" class="supports2">
							<li class="support-item" v-for="(value,index) of seller.supports">
								<span class="icon" :class="classMap[seller.supports[index].type]"></span>
								<span class="text">{{seller.supports[index].description}}</span>
							</li>
						</ul>
						<div class="detail-title">
							<div class="line"></div>
							<div class="detail-text">商家公告</div>
							<div class="line"></div>
						</div>
						<div class="detail-bulletin">
							<p class="content">{{seller.bulletin}}</p>
						</div>
					</div>
				</div>
				<div class="detail-close" @click="hideDetail">
					<i class="icon-close"></i>
				</div>
			</div>
		</transition>
	</div>
</template>
<script type="text/ecmascript-6">
	import star from '@/components/star/star'

	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data () {
			return {
				detailShow: false
			}
		},
		methods: {
			showDetail () {
				this.detailShow = true
			},
			hideDetail () {
				this.detailShow = false
			}
		},
		created () {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
		},
		components: {
			star
		}
	}
</script>
<style scoped>
	@import url(../../common/css/icon.css);
	.header {
		color: #fff;
		position: relative;
		background-color: rgba(7,17,27,0.5);
		overflow: hidden;
	}
	.content-wrapper {
		padding: 24px 12px 18px 24px;
		font-size: 0;
		position: relative;
	}
	.avatar {
		display: inline-block;
		vertical-align: top;
		border-radius: 2px;
		overflow: hidden;
	}
	.content {
		display: inline-block;
		margin-left: 16px;
	}
	.title {
		margin: 2px 0 8px 0;
	}
	.brand {
		width: 30px;
		height: 18px;
		display: inline-block;
		background: url(brand@2x.png) no-repeat;
		background-size: 30px 18px;
		vertical-align: top;
	}
	.name {
		margin-left: 6px;
		font-size: 16px;
		line-height: 18px;
		font-weight: bold;

	}
	.desc {
		margin-bottom: 10px;
		line-height: 12px;
		font-size: 12px;

	}
	.supports .icon {
		display: inline-block;
		width: 12px;
		height: 12px;
		margin-right: 4px;
		background-size: 12px;
		background-repeat: no-repeat;
		vertical-align: top;
	}
	.supports .decrease {
		background-image: url(decrease_1@2x.png);
	}
	.supports .discount {
		background-image: url(discount_1@2x.png);
	}
	.supports .guarantee {
		background-image: url(guarantee_1@2x.png);
	}
	.supports .invoice {
		background-image: url(invoice_1@2x.png);
	}
	.supports .special {
		background-image: url(special_1@2x.png);
	}
	.text {
		font-size: 10px;
		line-height: 12px;
	}
	.support-count {
		position: absolute;
		right: 12px;
		bottom: 14px;
		padding: 0 8px;
		height: 24px;
		line-height: 24px;
		border-radius: 14px;
		background-color: rgba(0,0,0,0.2);
		text-align: center;
	}
	.count {
		font-size: 10px;
		vertical-align: top;
	}
	.icon-keyboard_arrow_right {
		font-size: 10px;
		line-height: 24px;
		margin-left: 2px;
	}
	.bulletin-wrapper {
		position: relative;
		height: 28px;
		font-size: 0;
		line-height: 28px;
		padding: 0 22px 0 12px;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		background-color: rgba(7,17,27,0.2);
	}
	.bulletin-wrapper .icon-keyboard_arrow_right {
		position: absolute;
		font-size: 10px;
		right: 12px;
		top: 6px;
	}
	.bulletin-title {
		display: inline-block;
		width: 22px;
		height: 12px;
		vertical-align: top;
		margin-top: 7px;
		background-image: url(bulletin@2x.png);
		background-size: 22px 12px;
		background-position: center;
	}
	.bulletin-text {
		font-size: 10px;
		font-weight: 200;
		margin: 0 4px;
	}
	.background {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: -1;
		filter: blur(10px);
	}
	.detail {
		position: fixed;
		top: 0;
		left: 0;
		z-index: 100;
		width: 100vw;
		height: 100vh;
		overflow: auto;
		background-color: rgba(7,17,27,0.4);
		backdrop-filter: blur(10px);
	}
	.detail-wrapper {
		width: 100%;
		min-height: 100vh;
	}
	.main {
		padding-top: 64px;
		padding-bottom: 64px;
	}
	.detail-close {
		position: relative;
		width: 32px;
		height: 32px;
		margin: -64px auto 0;
		clear: both;
		font-size: 32px;
		z-index: 101;
	}
	.detail-name {
		line-height: 16px;
		font-size: 16px;
		font-weight: 700;
		text-align: center;
	}
	.star-wrapper {
		margin-top: 18px;
		padding: 2px 0;
		text-align: center;
	}
	.detail-title {
		display: flex;
		width: 80%;
		margin: 28px auto 24px;
	}
	.detail-title .line {
		flex: 1;
		position: relative;
		top: -6px;
		border-bottom: 1px solid rgba(255,255,255,0.2);
	}
	.detail-text {
		padding: 0 12px;
		font-size: 14px;
		font-weight: 700

	}
	.supports2 {
		width: 80%;
		margin: 0 auto;
	}
	.supports2 .support-item {
		height: 16px;
		padding: 0 12px;
		margin-bottom: 12px;
		font-size: 0;
	}
	.supports2 .support-item:last-child {
		margin-bottom: 0;
	}
	.supports2 .icon {
		display: inline-block;
		width: 16px;
		height: 16px;
		margin-right: 6px;
		background-size: 16px;
		background-repeat: no-repeat;
		vertical-align: top;
	}
	.supports2 .decrease {
		background-image: url(decrease_2@2x.png);
	}
	.supports2 .discount {
		background-image: url(discount_2@2x.png);
	}
	.supports2 .guarantee {
		background-image: url(guarantee_2@2x.png);
	}
	.supports2 .invoice {
		background-image: url(invoice_2@2x.png);
	}
	.supports2 .special {
		background-image: url(special_2@2x.png);
	}
	.supports2 .text {
		line-height: 16px;
		font-size: 12px;
	}
	.detail-bulletin {
		width: 80%;
		margin: 0 auto
	}
	.detail-bulletin .content {
		padding: 0 12px;
		line-height: 24px;
		font-size: 12px;
		margin-left: 0;
	}
	/*过渡动画*/
	.fade-enter-active {
	  transition: all .3s ease;
	}
	.fade-leave-active {
	  transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
	}
	.fade-enter, .fade-leave-active {
	  opacity: 0;
	}
</style>
