<template>
	<view class="content">
		<view class="size">富文本解析：</view>
		<view v-html="nodes"/>
		
		<view class="size">wxParse：之前uni-app不支持v-html的解决方案</view>
		<wxParse :content="nodes"/>
	</view>
</template>

<script>
	import wxParse from '@/components/mpvue-wxparse/src/wxParse.vue';
	
	export default {
		components: {
			wxParse,
		},
		data() {
			return {
				nodes: '<p>啦啦啦</p><div style="color: red;">div呀</div>',
			};
		}
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
