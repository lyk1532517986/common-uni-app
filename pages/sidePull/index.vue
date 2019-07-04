<template>
	<view class="content">
		<view class="size">
			筛选样式1：简单筛选（单选和多选）。
		</view>
		<view class="btn" @click="navTo('/pages/sidePull/sidePull1')">
			筛选样式1
		</view>
		<view class="size">
			筛选样式2：除了简单筛选（单选）和自定义其他选择。
		</view>
		<view class="btn" @click="navTo('/pages/sidePull/sidePull2')">
			筛选样式2
		</view>
	</view>
</template>
<script>
	export default {
		methods: {
			navTo(path){
				uni.navigateTo({
					url: path
				})
			},
		},
		onHide() {
			console.log('onHide隐藏侧拉')
		},
		beforeDestroy() {
			console.log('beforeDestroy隐藏侧拉')
		},
		destroyed() {
			console.log('destroyed隐藏侧拉')
		},
	}
</script>

<style>
	.content {
		padding: 25upx;
		background-color: #eee;
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
	.size {
		font-size: 26upx;
		line-height: 37upx;
		color: #666;
	}
</style>
