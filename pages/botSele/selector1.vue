<template>
	<view class="content">
		<button @click="showPicker">单列选择</button>
		<mpvue-picker 
			ref="mpvuePicker" 
			:mode="mpvuePicker.mode" 
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
					mode: 'selector',
					pickerValueArray: [
						{
							label: '住宿费',
							value: 1
						},
						{
							label: '活动费',
							value: 2
						},
						{
							label: '通讯费',
							value: 3
						},
						{
							label: '补助',
							value: 4
						}
					],
					pickerValueDefault: [1]
				}
			}
		},
		methods: {
			showPicker() {
				this.$refs.mpvuePicker.show();
			},
			onConfirm(e) {
				console.log('确认')
				console.log(e);
			},
			onChange(e) {
				console.log(e);
			},
			onCancel(e) {
				console.log('取消')
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