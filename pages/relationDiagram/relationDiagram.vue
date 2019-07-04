<template>
	<view class="content">
		<view class="size">饼状图</view>
		<view class="echarts-box">
			<view v-if="!stockPath" class="chart-tips">
				正在生成中...
			</view>
			<image v-if="stockPath" :src="stockPath" style="width: 100%;" mode="widthFix"></image>
			<view v-if="!stockPath" class="chart-abs">
				<mpvue-echarts param="stockOption" :echarts="echarts" :onInit="echartsInit" canvasId="stock" ref="stockChart" />
			</view>
		</view>
		<view class="size">饼状图2</view>
		<view class="echarts-box">
			<view v-if="!stockPath2" class="chart-tips">
				正在生成中...
			</view>
			<image v-if="stockPath2" :src="stockPath2" style="width: 100%;" mode="widthFix"></image>
			<view v-if="!stockPath2" class="chart-abs">
				<mpvue-echarts param="stockOption2" :echarts="echarts" :onInit="echartsInit" canvasId="stock2" ref="stockChart2" />
			</view>
		</view>
		<view class="size">饼状图3</view>
		<view class="echarts-box">
			<view v-if="!stockPath3" class="chart-tips">
				正在生成中...
			</view>
			<image v-if="stockPath3" :src="stockPath3" style="width: 100%;" mode="widthFix"></image>
			<view v-if="!stockPath3" class="chart-abs">
				<mpvue-echarts param="stockOption3" :echarts="echarts" :onInit="echartsInit" canvasId="stock3" ref="stockChart3" />
			</view>
		</view>
		<view class="size clr">
			注意：<br/>
			1、使用图片的原因是因为原生组件高于一切。如果有弹框。弹框无法覆盖原生组件的高度。使用z-index也不可以。
		</view>
	</view>
</template>

<script>
	import * as echarts from '@/components/echarts/echarts.min.js';
	import mpvueEcharts from '@/components/mpvue-echarts/src/echarts.vue';
	export default {
		components: {
			mpvueEcharts,
		},
		data() {
			return {
				echarts: echarts,
				
				stockPath: '',
				stockOption: {
					series: [
						{
						  name: '传播力',
						  type: 'tree',
						  initialTreeDepth: -1,//节点是否都展开
						  
						  expandAndCollapse: false,//是否可以折叠
						  roam: true,//可拖拽
						
						  data: [],
						  top: '5%',
						  left: '15%',
						  bottom: '2%',
						  right: '10%',
						  symbolSize: 10,
						  symbol: 'circle',
							
						  label: {
							normal: {
								rotate: '90',
								position: 'left',
								verticalAlign: 'bottom',
								align: 'center',
								color: 'black'
							}
						  }
							
						},
					]
				},
				
				
				
				stockPath2: '',
				stockOption2: {
					series : [
						{
							type: 'pie',
							hoverAnimation: false,
							legendHoverLink: false,
							animation: false,
							label: {
								show: false,
							},
							radius : '100%',
							center: ['50%', '50%'],
							data:[],//数据在使用的时候去赋值的。上面只是配置样式
						}
					]
				},
				
				
				stockPath3: '',
				stockOption3: {
					series : [
						{
							type: 'pie',
							hoverAnimation: false,
							legendHoverLink: false,
							animation: false,
							label: {
								show: false,
							},
							radius : '100%',
							center: ['50%', '50%'],
							data:[],//数据在使用的时候去赋值的。上面只是配置样式
						}
					]
				},
				
			};
		},
		onLoad() {
			this.initAjax();
		},
		methods: {
			initAjax(){
				const vm = this;
				// 模拟接口返回数据canvas重新绘图
				vm.timers = setTimeout(()=>{
					const res = [
						{
							"name": "陈永辉",
							"children": [
								{
									"name": "冯俊男",
									"children": [
										{
											name: "魏江龙1",
											children: [],
										},
									]
								}
							]
						}
					];
					// 、、、、、、折线图1
					//数据赋值
					vm.stockOption.series[0].data = res;
					vm.echartsRender('stockChart', 'stockPath');
					
					
					const res2 = [
						{
							value: 10,
							name: '水',
							itemStyle: {
								color: '#975FE4'
							}
						},
						{
							value:2,
							name: '电',
							itemStyle: {
								color: '#3AA0FF'
							}
						},
						{
							value:300,
							name: '木',
							itemStyle: {
								color: '#4ECB73'
							}
						},
						{
							value:4,
							name: '瓦',
							itemStyle: {
								color: '#F5A623'
							}
						},
						{
							value:50,
							name: '油',
							itemStyle: {
								color: '#435188'
							}
						},
					];
					// 、、、、、、折线图1
					//数据赋值
					vm.stockOption2.series[0].data = res2;
					vm.echartsRender('stockChart2', 'stockPath2');
					
					
					const res3 = [
						{
							value: 100,
							name: '水',
							itemStyle: {
								color: '#975FE4'
							}
						},
						{
							value:10,
							name: '电',
							itemStyle: {
								color: '#3AA0FF'
							}
						},
						{
							value:3,
							name: '木',
							itemStyle: {
								color: '#4ECB73'
							}
						},
						{
							value:4,
							name: '瓦',
							itemStyle: {
								color: '#F5A623'
							}
						},
						{
							value:50,
							name: '油',
							itemStyle: {
								color: '#435188'
							}
						},
					];
					// 、、、、、、折线图1
					//数据赋值
					vm.stockOption3.series[0].data = res3;
					vm.echartsRender('stockChart3', 'stockPath3');
				},1000);
			},
			
			echartsRender(refs,imgUrl){
				const vm = this;
				const refsDom = vm.$refs[refs];
				const pro = refsDom.init();
			},
			// 饼状图生成
			echartsInit(canvas, width, height, param) {
				const vm = this;
				let chart = echarts.init(canvas, null, {
					width: width,
					height: height,
				});
				canvas.setChart(chart);
				chart.setOption(vm[param]);
				return chart;
			},
		}
	}
</script>
<style>
	.clr {
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
	.btn {
		width: 236upx;
		height: 77upx;
		text-align: center;
		line-height: 77upx;
		background-color: #F5333D;
		color: #fff;
		margin: 50upx auto;
	}
	
</style>
<style lang="less" scoped>
	.echarts-box {
		position: relative;
		width: 500upx;
		height: 500upx
	}
	.chart-tips {
		position: absolute;
		left: 0;
		top: 50%;
		margin-top: -20upx;
		width: 100%;
		height: 40upx;
		line-height: 40upx;
		font-size: 26upx;
		text-align: center;
		color: #ccc;
	}
	.chart-abs {
		position: absolute;
		width: 100%;
		height: 100%;
		// left: -100%;
		// top: -100%;
	}
</style>
