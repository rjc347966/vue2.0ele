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
					
				</div>
			</div>
		</div>
	</div>
</template>
<script type="text/ecmascript-6">
	export default {
		props: {
			seller: {
				type: Object
			}
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
</style>
