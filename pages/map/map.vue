<template>
	<view style="width: 100%; height: 100%;">
		<view class="myMap" style="width: 100%; height: 100%;" >
			<map id="myMap" style="width: 100%; height: 100%;" 
				:latitude="latitude" 
				:longitude="longitude" 
				:markers="covers"
				:polyline="polyline"
			/>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'map',
				latitude: 39.909,
				longitude: 116.39742,
				covers: [],
				polyline: []
			};
		},
		onLoad() {
			const vm = this;
			vm.mapInit();
		},
		methods: {
			mapInit() {
				const vm = this;
				const mapContext = uni.createMapContext("myMap");
				vm.covers = [{
					latitude: 39.909,
					longitude: 116.39742,
					iconPath: '../../static/location3.png',
					callout: {
						content: '哈哈',
						color: '#69CA81',
						fontSize: 20,
						bgColor: '#F23D00'
					}
				}, {
					latitude: 39.90,
					longitude: 116.39,
					iconPath: '../../static/location3.png'
				}];
				
				
				vm.polyline = [{
					points: [{
						latitude: 39.909,
						longitude: 116.39742,
						content: '这里是时间'
					},
					{
						latitude: 39.90,
						longitude: 116.39,
						content: '啦啦'
					}],
					color: "#FF4500",
					width: 3,
					dottedLine: true,
					arrowLine: true
				}]
				
				mapContext.includePoints({
					points: [{latitude: 39.909,longitude: 116.39742},{latitude: 39.90,longitude: 116.39}]
				});
			}
		},
	}
</script>
<style>
	page {
		width: 100%; height: 100%;
	}
</style>