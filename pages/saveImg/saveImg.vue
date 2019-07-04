<template>
	<view>
		<view>需要合法域名配置</view>
		<view @click="saveImg()">保存图片</view>
		<view class="e">
			fail1:{{fail1}} <br/>
			fail2:{{fail2}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				fail1: '',
				fail2: '',
			};
		},
		methods: {
			saveImg(){
				const vm = this;
				uni.downloadFile({
					url: 'https://csm.soufucai.com/Static/img/logo.png', //仅为示例，并非真实的资源
					success: (res) => {
						if (res.statusCode === 200) {
							console.log('下载成功');
							console.log('临时路径',res.tempFilePath);
							uni.saveImageToPhotosAlbum({
								filePath: res.tempFilePath,
								success: function () {
									uni.showToast({
										title: '保存成功',
										duration: 2000
									});
								},
								fail: function(e){
									vm.fail2 = JSON.stringify(e)
									console.log(JSON.stringify(e))
									uni.showToast({
										title: '保存失败',
										duration: 2000
									});
								}
							});
						};
					},
					fail: function(e){
						vm.fail1 = JSON.stringify(e)
						console.log(JSON.stringify(e))
						uni.showToast({
							title: '下载失败',
							duration: 2000
						});
					}
				});
			},
		},
	}
</script>

<style lang="less">

</style>
