<template>
	<view class="nav-page" :style="setColorStyle">
		<cover-view class="bottom-nav">
			<cover-view class="nav-same" :class="{active: current == nub}" :style="current == nub ? setActiveFontColorStyle : ''" v-for="(item, nub) in list" :key="nub" @click="navTo(nub)">
				<!-- <cover-view class="nav-rq"> -->
					<cover-image class="nav-icon" :src="select == nub ? item.srcSelect : item.src" mode="aspectFit"/>
					<cover-view class="nav-title">{{item.text}}</cover-view>
				<!-- </cover-view> -->
			</cover-view>
		</cover-view>
	</view>
</template>

<script>
	export default {
		name: 'cmd-bottom-nav',
		props: {
			/**
			* 导航列表选中项
			*/
			current: {
				type: Number,
				default: 0
			},
			/**
			* 导航列表
			*/
			list: {
				type: Array,
				default: () => {
					return [
						{
							"pagePath": "/pages/index/index",
							"text": "组件", 
							"src": "../../static/home.png",
							"srcSelect": "../../static/homeactive.png"
						},
						{
							"pagePath": "/pages/template/template",
							"text": "模板",
							"src": "../../static/business.png",
							"srcSelect": "../../static/businessactive.png"
						}
					];
				}
			},
			/**
			 * 底部上边线颜色
			 */
			borderColor: {
			  type: String,
			  default: ''
			},
			/**
			* 文字颜色
			*/
			fontColor: {
				type: String,
				default: ''
			},
			/**
			* 背景颜色
			*/
			backgroundColor: {
				type: String,
				default: ''
			},
			/**
			* 激活文字颜色
			*/
			activeFontColor: {
				type: String,
				default: ''
			},
			// 只在激活状态显示文本
			textAuto: {
				type: Boolean,
				default: false
			},
			// 固定到页面底部
			fixed: {
				type: Boolean,
				default: true
			}
		},

		data() {
			return {
				userInfo: {},//178 ==  销售总监
				// 选中项
				select: this.current
			};
		},

		computed: {
			/**
			* 底部导航栏颜色样式
			*/
			setColorStyle() {
				let colorStyle = '';
				// 文字颜色
				if (this.fontColor != '') {
				  colorStyle += `color:${this.fontColor};`;
				}
				// 上边线颜色
				if (this.borderColor != '') {
				  colorStyle += `border-top: 1px ${this.borderColor} solid;`;
				}
				// 背景颜色
				if (this.backgroundColor != '') {
				  colorStyle += `background: ${this.backgroundColor};`;
				}
				return colorStyle;
			},
			/**
			* 激活文字样式
			*/
			setActiveFontColorStyle() {
				let activeFontColorStyle = '';
				if (this.activeFontColor != '') {
				  activeFontColorStyle += `color:${this.activeFontColor};`;
				}
				return activeFontColorStyle;
			}
		},
		onLoad() {
			this.userInfo = uni.getStorageSync('userInfo');
			if(this.userInfo){
				this.userInfo = JSON.parse(this.userInfo);
			}else {
				console.log('没有获取到登录信息的话，这里要回跳到登录页面');
			};
			
			this.initPage();
		},
		methods: {
			initPage(){
				const vm = this;
				
			},
			/**
			* 点击事件
			*/
			navTo(index) {
				this.select = index;
				if(this.current != index){
					uni.redirectTo({
						url: this.list[index].pagePath
					})
				}
			}
		}

	}
</script>

<style lang="less" scoped>
	
	.nav-page {
		position: fixed;
		left: 0;
		bottom: 0;
		width: 100%;
		height: 100upx;
		// box-sizing: content-box;
	}
	.bottom-nav {
		display: flex;
		position: fixed;
		left: 0;
		bottom: 0;
		width: 100%;
		height: 100upx;
		padding-top: 12upx;
		box-sizing: border-box;
		
		.nav-same {
			flex: 1;
			flex-direction:column;
			text-align: center;
			// flex-direction:column;
			// display: flex;
			// align-items: center;
			
			.nav-rq {
				width: 100%;
				height: 100%;
			}
			
			.nav-icon {
				width: 44upx;
				height: 44upx;
				margin: 0 auto;
			}
			.nav-title {
				font-size: 28upx;
				line-height: 44upx;
			}
		}
	}
	
</style>
