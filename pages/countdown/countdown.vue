<template>
	<view class="content">
		<uni-countdown :timer="countdown.timer"/>
		<view class="size">
			注意：<br/>
			时间格式必须是：yyyy-yy-yy xx:xx:xx
		</view>
	</view>
</template>

<script>
	import uniCountdown from '@/components/uni-countdown.vue';
	export default {
		components: {
			uniCountdown,
		},
		data() {
			return {
				countdown: {
					timer: '2019-04-24 00:00:00',
				}
			};
		}
	}
</script>

<style>
	.clr {
		color: red;
	}
	.content {
		padding: 25upx;
		background-color: #eee;
	}
	.size {
		font-size: 26upx;
		line-height: 37upx;
		color: #666;
	}
	.btn {
		width: 236upx;
		height: 77upx;
		text-align: center;
		line-height: 77upx;
		background-color: #F5333D;
		color: #fff;
		margin: 50upx auto;
	}
	
</style>
