<template>
	<view class="content">
		<button @click="showPicker">多列选择</button>
		<view class="size">
			注意：<br/>
			1、目前是三列选择。如果需要2列选择<br/>
				1>pickerValueArray  里面删除 有“歼20”的那个数组。<br/>
				2>pickerValueDefault 修改为  [1, 1]
		</view>
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
					mode: 'multiSelector',
					pickerValueArray: [
						[
							{
								label: '中国',
								value: 1 ,
							},
							{
								label: '美国', 
								value: 2 ,
							},
							{
								label: '日本', 
								value: 3 ,
							},
							{
								label: '俄罗斯',
								value: 4,
							}
						],
						[
							{
								label: '茶', 
								value: 1 ,
							},
							{
								label: '咖啡', 
								value: 2 ,
							},
							{
								label: '寿司', 
								value: 3 ,
							},
							{
								label: '奶酪', 
								value: 4 ,
							}
						],
						[
							{
								label: '歼20', 
								value: 1 ,
							},
							{
								label: 'F22', 
								value: 2 ,
							},
							{
								label: '秋月级', 
								value: 3 ,
							},
							{
								label: 'T50', 
								value: 4 ,
							}
						]
					],
					pickerValueDefault: [1, 1, 1]
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