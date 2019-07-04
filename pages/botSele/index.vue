<template>
	<view class="content">
		<view class="size">1、单列选择器</view>
		<view class="btn" @click="navTo('/pages/botSele/selector1')">
			单列选择器
		</view>
		<view class="size">2、多列选择</view>
		<view class="btn" @click="navTo('/pages/botSele/selector2')">
			多列选择
		</view>
		<view class="size">3、二级联动</view>
		<view class="btn" @click="navTo('/pages/botSele/selector3')">
			二级联动
		</view>
		<view class="size">4、三级联动</view>
		<view class="btn" @click="navTo('/pages/botSele/selector4')">
			三级联动
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods: {
			navTo(path){
				uni.navigateTo({
					url: path
				})
			}
		}
	}
</script>

<style lang="less">
	.clr {
		color: red!important;
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
		padding: 0 40upx;
		height: 77upx;
		text-align: center;
		line-height: 77upx;
		background-color: #F5333D;
		color: #fff;
		margin: 50upx auto;
	}
</style>