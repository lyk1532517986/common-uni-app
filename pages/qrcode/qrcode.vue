<template>
	<view class="content">
		<view class="size">生成二维码</view>
		<view style="width: 200upx;height: 200upx;">
			<qrCode
				val="https://www.baidu.com/" 
				:size="200"/>
		</view>
		<view :style="{width: '100upx',height: '100upx'}">
			<qrCode
				val="https://www.baidu.com/" 
				:size="110"/>
		</view>
	</view>
</template>

<script>
	import qrCode from "@/components/qrcode/qrcode.vue";
	
	export default {
		components: {
			qrCode,
		},
		onLoad() {
			console.log(qrCode)
		},
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="less">

</style>
