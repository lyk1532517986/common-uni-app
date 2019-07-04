<template>
	<view class="content">
		<button @click="showPicker">二级联动</button>
		<mpvue-picker 
			ref="mpvuePicker"
			:mode="mpvuePicker.mode" 
			:deepLength='mpvuePicker.deepLength'
			:pickerValueDefault="mpvuePicker.pickerValueDefault" 
			:pickerValueArray="mpvuePicker.pickerValueArray"
			@onChange="onChange" 
			@onConfirm="onConfirm" 
			@onCancel="onCancel"/>
	</view>
</template>

<script>
	import mpvuePicker from '@/components/mpvue-picker/mpvuePicker.vue';
	export default {
		components: {
			mpvuePicker
		},
		data() {
			return {
				mpvuePicker: {
					mode: 'multiLinkageSelector',
					deepLength: 2,
					pickerValueArray: [
						{
						  label: 'phone',
						  value: 0,
						  children: [
							{
							  label: 'iphone',
							  value: 1,
							  children: [
								{
								  label: 'iphoneX',
								  value: 1
								},
								{
								  label: 'iphone8',
								  value: 2
								},
								{
								  label: 'iphone8 Plus',
								  value: 3
								}
							  ]
							},
							{
							  label: 'android',
							  value: 1,
							  children: [
								{
								  label: 'vivo',
								  value: 1
								},
								{
								  label: '魅族',
								  value: 2
								},
								{
								  label: '小米',
								  value: 3
								}
							  ]
							}
						  ]
						},
						{
						  label: 'PC',
						  value: 0,
						  children: [
							{
							  label: 'mac',
							  value: 1,
							  children: [
								{
								  label: 'macbook Pro',
								  value: 1
								},
								{
								  label: 'iMac',
								  value: 2
								},
								{
								  label: 'mackbook',
								  value: 3
								},
								{
								  label: 'mackbook air',
								  value: 3
								}
							  ]
							},
							{
							  label: 'windows',
							  value: 1,
							  children: [
								{
								  label: 'dell',
								  value: 1
								},
								{
								  label: 'surface',
								  value: 2
								},
								{
								  label: 'thinkpad',
								  value: 3
								}
							  ]
							}
						  ]
						}
					  ],
					pickerValueDefault: [0, 0]
				}
			}
		},
		methods: {
			showPicker() {
				this.$refs.mpvuePicker.show();
			},
			onConfirm(e) {
				console.log(e);
			},
			onChange(e) {
				console.log(e);
			},
			onCancel(e) {
				console.log(e);
			}
		},
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