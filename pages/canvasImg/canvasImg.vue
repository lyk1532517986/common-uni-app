<template>
	<view>
		<canvas canvas-id='firstCanvas' style='width: 700px;height:350px' @error="canvasIdErrorCallback"/>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		onReady(){
			var context = uni.createCanvasContext('firstCanvas');
			context.setStrokeStyle("#00ff00")
			context.setLineWidth(5)
			context.rect(0, 0, 200, 200)
			context.stroke()
			context.setStrokeStyle("#ff0000")
			context.setLineWidth(2)
			context.moveTo(160, 100)
			context.arc(100, 100, 60, 0, 2 * Math.PI, true)
			context.moveTo(140, 100)
			context.arc(100, 100, 40, 0, Math.PI, false)
			context.moveTo(85, 80)
			context.arc(80, 80, 5, 0, 2 * Math.PI, true)
			context.moveTo(125, 80)
			context.arc(120, 80, 5, 0, 2 * Math.PI, true)
			context.stroke()
			context.draw()
			console.log('画图乐乐2222')
			context.draw(true,()=>{
				console.log('huotule1111')
				uni.canvasToTempFilePath({
					x: 0,
					y: 0,
					width: 700,
					height: 350,
					destWidth: 700,
					destHeight: 350,
					canvasId: 'firstCanvas',
					success: function(res) {
						console.log('生成了')
						uni.showModal({
							title: '生成成功!',
							content: '路径：'+res.tempFilePath,
							success: function (res) {
								if (res.confirm) {
									console.log('用户点击确定');
								} else if (res.cancel) {
									console.log('用户点击取消');
								}
							}
						});
					} 
				})
			});
		},
		methods: {
			canvasIdErrorCallback: function (e) {
				console.error(e.detail.errMsg)
			}
		},
	}
</script>

<style lang="less">

</style>
