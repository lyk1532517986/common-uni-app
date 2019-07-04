<template>
	<view style="width: 100%; height: 100%;">
		<view class="myMap" style="width: 100%; height: 100%;" >
			<!-- :polyline="polyline" -->
			<map id="myMap" style="width: 100%; height: 100%;" 
				:latitude="latitude" 
				:longitude="longitude" 
				:markers="covers"
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
				vm.covers = [
					{
						latitude: 39.909,
						longitude: 116.39742,
						iconPath: '../../static/logo.png',
						width:20,
						height:20,
						callout: {
							content: '哈哈',
							color: '#69CA81',
							fontSize: 20,
							bgColor: '#fff',
							borderRadius:4,
							padding:15,
							display:'BYCLICK',
							textAlign:'left'
						},
						// label:{
						// 	content: '哈哈',
						// 	color: '#69CA81',
						// 	fontSize: 15,
						// 	x: 39.909,
						// 	y: 116.39742,
						// 	borderWidth: 1,
						// 	borderColor: '#eee',
						// 	borderRadius: 4,
						// 	bgColor: '#fff',
						// 	padding: 20,
						// 	textAlign: 'left',
						// }
					}, 
					{
						latitude: 39.90,
						longitude: 116.39,
						iconPath: '../../static/logo.png',
						width:20,
						height:20,
						callout: {
							content: '哈哈11',
							color: '#69CA81',
							fontSize: 20,
							bgColor: '#fff',
							borderRadius:4,
							padding:15,
							display:'BYCLICK',
							textAlign:'left'
						},
						
					},
					{
						latitude: 39.99,
						longitude: 116.45,
						iconPath: '../../static/logo.png',
						width:20,
						height:20,
						callout: {
							content: '哈哈22',
							color: '#69CA81',
							fontSize: 20,
							bgColor: '#fff',
							borderRadius:4,
							padding:15,
							display:'BYCLICK',
							textAlign:'left'
						},
					},
				];
				
				
				mapContext.includePoints({
					points: [{latitude: 39.909,longitude: 116.39742,content:'哈哈'},{latitude: 39.90,longitude: 116.39,content:'哈哈11'},{latitude: 39.99,longitude: 116.45,content:'哈哈22'}]
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