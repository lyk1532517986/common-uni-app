<template>
	<view class="content">
        <text>通用组件</text>
		<view class="box">
			<view class="same" @click="qiehuan()">
				底部导航切换为flex
			</view>
			<view class="same" v-for="(item, nub) in  list" :key="nub" @click="navTo(item.path)">
				{{item.title}}
			</view>
        </view>
		<cmd-bottom-nav
			v-if="navStyle"
			:current="current"
			border-color="#E5E5E5" 
			background-color="#fff" 
			font-color="#999" 
			active-font-color="#0079F3"/>
		<coverViewBottomNav
			v-if="!navStyle"
			:current="current"
			border-color="#E5E5E5" 
			background-color="#fff" 
			font-color="#999" 
			active-font-color="#0079F3"/>
	</view>
</template>

<script>
	import cmdBottomNav from "@/components/cmd-bottom-nav/cmd-bottom-nav.vue";
	import coverViewBottomNav from "@/components/coverView-bottom-nav/coverView-bottom-nav";
	
	export default {
		components: {
			cmdBottomNav,
			coverViewBottomNav
		},
		data() {
			return {
				navStyle: true,
				current: 0,
				list: [
					{
						path: '/pages/leftDel/leftDel',
						title: '左滑删除'
					},
					{
						path: '/pages/sidePull/index',
						title: '侧拉筛选'
					},
					{
						path: '/pages/timeScreening/index',
						title: '时间选择器'
					},
					{
						path: '/pages/botSele/index',
						title: '底部弹出选择器'
					},
					{
						path: '/pages/search/index',
						title: '搜索框'
					},
					{
						path: '/pages/popup/index',
						title: '弹出层'
					},
					{
						path: '/pages/countdown/countdown',
						title: '倒计时'
					},
					{
						path: '/pages/richText/richText',
						title: '富文本解析'
					},
					{
						path: '/pages/qrcode/qrcode',
						title: '生成二维码'
					},
					{
						path: '/pages/echarts/echarts',
						title: '折线图'
					},
					{
						path: '/pages/saveImg/saveImg',
						title: '图片下载保存到手机相册(需要配置下载合法域名)'
					},
					{
						path: '/pages/canvasImg/canvasImg',
						title: 'canvas生成图片'
					},
					{
						path: '/pages/relationDiagram/relationDiagram',
						title: 'canvas生成关系图'
					},
					{
						path: '/pages/map/map',
						title: '路径轨迹图'
					},
					{
						path: '/pages/heatmap/heatmap',
						title: '仿热力图'
					},
					{
						path: '/pages/scanCode/scanCode',
						title: '微信API扫码'
					},
					{
						path: '/pages/saveImageToPhotosAlbum/saveImageToPhotosAlbum',
						title: '保存图片到本地'
					},
				]
			}
		},
		onHide() {
			console.log('onHide隐藏首页')
		},
		beforeDestroy() {
			console.log('beforeDestroy销毁前首页')
		},
		destroyed() {
			console.log('destroyed销毁首页')
		},
		methods: {
			qiehuan(){
				const vm = this;
				vm.navStyle ? vm.navStyle = false : vm.navStyle = true;
			},
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
