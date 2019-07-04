<template>
	<view class="content">
        <text>页面模板</text>
		<view class="box">
			<view class="same" v-for="(item, nub) in  list" :key="nub" @click="navTo(item.path)">
				{{item.title}}
			</view>
        </view>
		<cmd-bottom-nav
			:current="current"
			border-color="#E5E5E5" 
			background-color="#fff" 
			font-color="#999" 
			active-font-color="#0079F3"/>
	</view>
</template>

<script>
	import cmdBottomNav from "@/components/cmd-bottom-nav/cmd-bottom-nav.vue";
	
	export default {
		components: {
			cmdBottomNav
		},
		data() {
			return {
				current: 1,
				list: [
					{
						path: '/pages/pullUpLoading/pullUpLoading',
						title: '上拉加载'
					},
					{
						path: '/pages/dropDownRefresh/dropDownRefresh',
						title: '下拉刷新'
					}
				]
			}
		},
		onLoad() {
			
		},
		methods: {
			navTo(path){
				uni.navigateTo({
					url: path,
				})
			},
		}
	}
</script>

<style lang="less" scoped>
	.content {
		padding: 25upx 25upx 110px;
		background-color: #eee;
	}
	.box {
		margin-top: 20upx;
	}
	.same {
		margin-top: 20upx;
		font-size: 26upx;
		line-height: 37upx;
		background-color: #fff;
		color: #000;
		padding: 20upx;
	}
</style>
