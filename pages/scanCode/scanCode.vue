<template>
	<view class="content">
		<view class='form-list'>
			<text>单号：</text>
			<input type='text' class="uni-input" v-model="formDate.scanCodeMsg" @input="BindInput" data-val="scanCodeMsg"></input>
			<image class='scan' src='../../static/scanCode.png' mode='widthFix' @click="scanCode"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				formDate:{
					scanCodeMsg: ''
				}
			}
		},
		onLoad() {

		},
		methods: {
			BindInput(e){
				console.log(e);
				var vm=this;
				var dataval = e.currentTarget.dataset.val;
				vm.formDate[dataval] = e.detail.value; 
			},
			scanCode() {
				var vm = this;
				uni.scanCode({ //扫描API
				success(res) { //扫描成功
					console.log(res) //输出回调信息
					vm.formDate.scanCodeMsg = res.result;
					/*
					result	string	所扫码的内容
					scanType	string	所扫码的类型
					charSet	string	所扫码的字符集
					path	string	当所扫的码为当前小程序二维码时，会返回此字段，内容为二维码携带的 path
					rawData	string	原始数据，base64编码*/
					uni.showToast({
					title: '识别成功',
					duration: 2000
					})
				},
				fail(err) {
					console.log(err);
					uni.showToast({
						title: '识别失败',
						icon:"none",
						duration: 2000
					})
				}
				})
			},
			
		}
	}
</script>

<style lang="less">
	.content {
		.form-list{
			width: 750upx;
			height:80upx;
			position: relative;
			text{
				width: 150upx;
				display:block;
				height:80upx;
				line-height:80upx;
				float:left;
				text-align: center;
			}
			.uni-input{
				float:left;
				width: 600upx;
				border:1px solid #ddd;
				height:80upx;
				min-height:80upx;
				box-sizing: border-box;
				padding:0 15upx;
			}
			.scan{
				position: absolute;
				top:10upx;
				right:20upx;
				width: 60upx;
				height:56.8upx;
				z-index: 111;
			}
		}
	}
</style>
