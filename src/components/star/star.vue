<template>
	<div class="star" :class="starType">
		<div v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></div >
	</div>
</template>
<script type="text/ecmascript-6">
	const LENGTH = 5
	const CLS_ON = 'on'
	const CLS_OFF = 'off'
	const CLS_HALF = 'half'
	export default {
		props: {
			size: {
				type: Number
			},
			score: {
				type: Number
			}
		},
		computed: {
			starType () {
				return 'star-' + this.size
			},
			itemClasses () {
				let result = []
				let score = Math.floor(this.score * 2) / 2
				let hasDecimal = score % 1 !== 0
				let integar = Math.floor(score)
				for (let i = 0; i < integar; i++) {
					result.push(CLS_ON)
				}
				if (hasDecimal) {
					result.push(CLS_HALF)
				}
				while (result.length < LENGTH) {
					result.push(CLS_OFF)
				}
				return result
			}
		}
	}
</script>
<style scoped>
	.star {
		font-size: 0;
	}
	.star-24 {

	}
	.star-36 {
		
	}
	.star-48 {
		
	}
	.star:last-child {
		margin-right: 0;
	}
	.star-24 .star-item {
		display: inline-block;
		width: 10px;
		height: 10px;
		margin-right: 3px;
		background-size: 10px 10px
	}
	.star-24 .on {
		background-image: url(star24_on@2x.png);
	}
	.star-24 .half {
		background-image: url(star24_half@2x.png);
	}
	.star-24 .off {
		background-image: url(star24_off@2x.png);
	}
	.star-36 .star-item {
		display: inline-block;
		width: 15px;
		height: 15px;
		margin-right: 8px;
		background-size: 15px 15px
	}
	.star-36 .on {
		background-image: url(star36_on@2x.png);
	}
	.star-36 .half {
		background-image: url(star36_half@2x.png);
	}
	.star-36 .off {
		background-image: url(star36_off@2x.png);
	}
	.star-48 .star-item {
		display: inline-block;
		width: 20px;
		height: 20px;
		margin-right: 22px;
		background-size: 20px 20px
	}
	.star-48 .on {
		background-image: url(star48_on@2x.png);
	}
	.star-48 .half {
		background-image: url(star48_half@2x.png);
	}
	.star-48 .off {
		background-image: url(star48_off@2x.png);
	}
</style>
