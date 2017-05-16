<template>
	<div class="ratings">
		<div class="ratings-content">
			<div class="overview">
				<div class="overview-left">
					<h1 class="score">{{seller.score}}</h1>
					<div class="title">综合排名</div>
					<div class="rank">高于周边商家{{seller.rankRate}}%</div>
				</div>
				<div class="overview-right">
					<div class="score-wrapper">
						<span class="title">
							 服务态度
						</span>
						<star :size="36" :score="seller.serviceScore"></star>
						<span class="score">{{seller.serviceScore}}</span>
					</div>
					<div class="score-wrapper">
						<span class="title">
							 商品评分
						</span>
						<star :size="36" :score="seller.foodScore"></star>
						<span class="score">{{seller.foodScore}}</span>
					</div>
					<div class="delivery-wrapper">
						<span class="title">
							送达时间
						</span>
						<span class="delivery">{{seller.deliveryTime}}分钟</span>
					</div>
				</div>
			</div>
		</div>
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
		components: {
			star
		}
	}
</script>
<style>
	.ratings {
		position: absolute;
		top: 174px;
		bottom: 0;
		left: 0;
		width: 100%;
		overflow: hidden; 
	}
	.overview {
		display: flex;
		padding: 18px 0;
	}
	.overview-left {
		flex: 0 0 137px;
		width: 137px;
		padding: 6px 0;
		border-right: 1px solid rgba(7,17,27,0.1);
		text-align: center;
	}
	.overview-right {
		flex: 1;
		padding-left: 24px;
	}
	.overview-left .score {
		line-height: 28px;
		font-size: 24px;
		margin-bottom: 6px;
		color: rgb(255,153,0);
	}
	.overview-left .title {
		line-height: 12px;
		font-size: 12px;
		margin-bottom: 8px;
		color: rgb(7,17,27);
	}
	.overview-left .rank {
		font-size: 10px;
		line-height: 10px;
		color: rgb(147,153,159);
	}
	.score-wrapper, .delivery-wrapper {
		line-height: 18px;
		margin-bottom: 8px;
		font-size: 0;
	}
	.score-wrapper .title {
		 font-size: 12px;
		 line-height: 18px;
		 color: rgb(7,17,27);
	}
	.score-wrapper .star {
		display: inline-block;
		vertical-align: top;
		margin-left: 12px;
		margin-right: 4px;
	}
	.score-wrapper .score {
		font-size: 12px;
		vertical-align: top;
		line-height: 18px;
		color: rgb(255,153,0);
	}
	.delivery-wrapper .title {
		font-size: 12px;
		line-height: 18px;
		color: rgb(7,17,27);
		margin-right: 12px;
	}
	.delivery-wrapper .delivery {
		font-size: 12px;
		color: rgb(147,153,159);
		line-height: 18px;
	}
</style>
