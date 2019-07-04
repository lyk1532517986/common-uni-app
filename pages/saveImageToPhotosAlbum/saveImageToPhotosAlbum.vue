<template>
	<view>
		<view>保存网络图片到本地</view>
		<button @click="downloadImg">保存</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		},
		methods:{
			downloadImg(){
				var vm=this;
				var url = "http://imgs.soufucai.com/Uploads/goodsImg/thumb_img/68594_thumb_201905071732311557221551104_l.jpg";
				uni.downloadFile({
					url: url, //仅为示例，并非真实的资源
					success(res){
						console.log("downloadImg",res);
						if (res.statusCode === 200) {
							console.log('下载成功');
							
							vm.saveImageToPhotosAlbum(res.tempFilePath);
						}
					}
				});
			},
			saveImageToPhotosAlbum(filePath){
				uni.saveImageToPhotosAlbum({
					filePath:filePath,
					success(res) {
						console.log("saveImageToPhotosAlbum",res);

						if(res.errMsg == "saveImageToPhotosAlbum:ok"){
							console.log('save success');
							uni.showToast({
								title:'保存到系统相册成功！',
								duration:2000
							})
						}else{
							console.log('save fail');
							uni.showToast({
								title:'保存到系统相册失败！',
								duration:2000,
								icon:"none"
							})
						}
						
					}
				});
			}
		}
	}
</script>

<style lang="less">

</style>
