<template>
	<view class="content">
		<view class="size">
			点击时间进行选择
		</view>
		
		<ruiDatePicker
			fields="minute"
			start="2010-00-00 00:00"
			end="2030-12-30 23:59"
			:value="timeScrreen.scrrValue"
			@change="bindChange"
			@cancel="bindCancel"/>
			
		<view class="size">
			参数说明请查看：@/components/rattenking-dtpicker/readme.md
		</view>
		<view class="size">
			注意：<br/>
			1、scrrValue (默认当前显示那个时间) 必须是<view class="clr">yyyy-yy-yy xx:xx:xx</view> 格式。
		</view>
	</view>
</template>

<script>
	// 时间多级选择
	import ruiDatePicker from '@/components/rattenking-dtpicker/rattenking-dtpicker.vue';
	
	export default {
		components: {
			ruiDatePicker,
		},
		data() {
			//有约定时间
			const currentTime = new Date();
			const currentYear = currentTime.getFullYear();
			const currentMoth = currentTime.getMonth() + 1;
			const currentDate = currentTime.getDate();
			const currentHour = currentTime.getHours();
			const currentMinute = currentTime.getMinutes();
			function change(t) {
			    if (t < 10) {
			        return "0" + t;
			    } else {
			        return t;
			    }
			};
			//这是当前时间。注意：scrrValue 必须是yyyy-yy-yy xx:xx:xx 格式
			const currenTimeValue = currentYear+'-'+change(currentMoth)+'-'+change(currentDate)+' '+change(currentHour)+':'+change(currentMinute);

			return {
				timeScrreen: {
					scrrValue: currenTimeValue,//当前时间选择器显示的时间
				}
			};
		},
		methods: {
			bindChange(value){
				console.log(value)
				this.timeScrreen.scrrValue = value;
			},
			bindCancel(){
				console.log('取消选择了！');
			}
		},
	}
</script>
<style>
	.clr {
		display: inline-block;
		vertical-align: middle;
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
	
</style>
