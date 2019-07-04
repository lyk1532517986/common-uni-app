<template>
	<view class="qrcode" style="width: 100%;height: 100%;">
		<image class="image" v-if="img != ''" :src="img" style="width: 100%;height: 100%;"/>
	</view>
</template>
<script>
	import QR from "./qrcode.js";
	export default {
		name: 'number-box',
		props: {
			val: {
				type: String,
				default: ''
			},
			size:{
				type:Number,
				default:100
			}
		},
		data(){
			return{
				img:''
			}
		},
		onLoad(){
			this.creatQrcode();
		},
		methods: {
			creatQrcode(){
				console.log(this.val)
				let val = String(this.val)
				if(val == ''){
					return false
				}
				let img = QR.createQrCodeImg(val, {
					size: parseInt(this.size)
				})
				this.img = img;
			},
			clearQrcode(){
				this.img = '';
			}
		},
		watch:{
			size(newVal, oldVal){
				if(newVal != oldVal){
					this.size = newVal;
					this.creatQrcode()
				}
			}
		}
	}
</script>
<style>
	.qrcode{
		display: flex;
		justify-content: center;
	}
</style>
