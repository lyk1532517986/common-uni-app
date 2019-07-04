<template>
	<view class="s-page-wrapper is-100vh">
		<view class="sfc-logo">
			<view class="is-flex is-column is-justify-center  is-align-center is-height-100">
				<image src="https://wap.soufucai.com/img/XCX/sfc/static/logo.png" mode="aspectFit" class="logoimg"></image>
			</view>
			<view class="sfc-text">
				搜辅材一站式采购平台
			</view>
		</view>
		<view>注意：如果需要微信授权必须填写上 AppID 否则会授权失败</view>
		<view class="content">
			<view class="has-mglr-10 ">
				<view class="login-text">
					授权登录
				</view>
				<view class="login-content">
					<button class="loginBtn" open-type="getUserInfo" lang="zh_CN" @getuserinfo="onGotUserInfo">
						<image src="https://wap.soufucai.com/img/XCX/sfc/static/loginBtnImg.png" alt="" class="loginBtnImg"></image>
					</button>
					<view class="login-text">
						授权登录后即可继续进行比价操作
					</view>
				</view>
			</view>
		</view>
		
		<view class="otherLogin" @click="toOtherLogin" v-if="button_status>0">
			账号密码登录
		</view>
	</view>
</template>

<script>
	var dateUtils = require('../../common/util.js').dateUtils;

	export default {
		data() {
			return {
				sale_user_id:'',
				button_status:1,
				unionid:'',
				user_id:'',
				fromPage:'',
				region_id:'',
				product_id:'',
				type_detail:'',
			};
		},
		onLoad(event) {
			console.log(event);
			uni.redirectTo({
				url: "/pages/index/index"
			});
			// var vm=this;
			// vm.sale_user_id=uni.getStorageSync('saleUserId');
		},
		onShow() {
// 			var vm=this;
// 			vm.checkShowLogin1();
// 			vm.unionid = uni.getStorageSync('unionid');
// 			vm.user_id = uni.getStorageSync('user_id');
// 			vm.fromPage = uni.getStorageSync('fromPage');
// 			vm.region_id = uni.getStorageSync('region_id');
// 			vm.product_id = uni.getStorageSync('product_id');
// 			vm.type_detail = uni.getStorageSync('type_detail');
// 
// 			wx.checkSession({
// 				success(res) {
// 					if(vm.unionid&&vm.user_id){
// 						uni.redirectTo({
// 							url: "/pages/index/index"
// 						});
// 					}
// 				},
// 				fail(err) {
// 					console.log("checkSession.fail",err);
// 					
// 					if(vm.unionid&&vm.user_id){
// 						console.log('本地已经有 user_id 和 unionid数据可以直接跳转了')
// 						uni.redirectTo({
// 							url: "/pages/index/index"
// 						});
// 					} else {
// 						console.log('需要填写AppID才可以授权')
// 					}
// 				}
// 			});
// 			
			
			
		},
		methods:{
			onGotUserInfo2(e){
				console.log(e)
			},
			clickevent(){
				
			},
			onGotUserInfo(e) {
				console.log(e);
				var vm=this;
				
				uni.removeStorageSync('pwToLogin');
			
				if(e.detail.errMsg=="getUserInfo:ok"){
					wx.login({
						success(res) {
							if (res.code) {
								// 发起网络请求
								wx.getUserInfo({
									success(rfs) {
										console.log("getUserInfo",rfs);
										vm.codeLogin(res.code,rfs.iv,rfs.encryptedData);
									}
								})
							} else {
								console.log('登录失败！' + res.errMsg)
							}
						},
						fail(err){
							console.log(err);
							uni.setStorageSync('userImg', 'https://wap.soufucai.com/img/XCX/sfc/static/logo.png');
							uni.setStorageSync('userName', '未登录，请点击头像登录');
						}
					})
				}else{
					uni.showToast({
						title: '已取消授权',
						duration: 2000,
						icon:"none"
					});
				}
				
			},
			codeLogin(code,iv,encryptedData){
				console.log(code,iv,encryptedData);
				var vm=this;
				uni.request({
					url: dateUtils.baseUrl+'Integral/api/getXcxUserInfo',
					data: {
						code:code,
						iv:iv,
						encryptedData:encryptedData
					},
					success:function(res) {
						console.log("getXcxUserInfo",res);
						if(res.data.status>0){
							uni.setStorageSync('unionid', res.data.data.unionId);
							uni.setStorageSync('userImg', res.data.data.avatarUrl);
							uni.setStorageSync('userName', res.data.data.nickName);
							uni.setStorageSync('openId', res.data.data.openId);
							vm.toDoLogin(res.data.data.unionId);
						}
					}
				});
			},
			toDoLogin(unionid){
				var vm=this;
				uni.request({
					url: dateUtils.baseUrl+'Integral/api/userLogin',
					data: {
						froms:'xcx',
						wxid:unionid,
						sale_user_id:vm.sale_user_id
					},
					success: (res) => {
						console.log(res);
						if(res.data.code==23){
							var region_id=res.data.data.region_id;
							var user_id=res.data.data.user_id;
							var mobile_phone=res.data.data.mobile_phone;
							uni.setStorageSync('userImg', res.data.data.headimg);
							uni.setStorageSync('cipher_text', res.data.data.cipher_text);
							uni.setStorageSync('mobile_phone', res.data.data.mobile_phone);
							uni.setStorageSync('region_id', res.data.data.region_id);
							uni.setStorageSync('region_name', res.data.data.region_name);
							uni.setStorageSync('user_id', res.data.data.user_id);
							uni.setStorageSync('userName', res.data.data.nickname);
							uni.setStorageSync('user_token', res.data.data.user_token_data.user_token);
							uni.setStorageSync('is_sale', res.data.data.is_sale);
							
							
							uni.showToast({
								title: '登录成功',
								duration: 2000,
								icon:"success"
							});
							setTimeout(function(){
								uni.redirectTo({
									url: "/pages/index/index"
								});
							},2000)
						}else{
							uni.showToast({
								title: '授权登录成功，请绑定手机号',
								duration: 2000,
								icon:"success"
							});
							uni.redirectTo({
								url: "/pages/index/index"
							});
						}
					}
				});
			},
			toOtherLogin(){
				uni.navigateTo({
					url: "/pages/login1/login1"
				})
			},
			checkShowLogin1(){
				var vm=this;
				uni.request({
					url: dateUtils.baseUrl+'Integral/api/setButtonStatus',
					success(res) {
						vm.button_status=res.data.button_status;
					},
					fail(err) {
						console.log(err);
					}
				});
			},
		}
	}
</script>

<style lang="less" scoped>
	@import "../../common/simplepro.css";
	
	page {
		height: calc(100% - 44px);
		background-color: #FFFFFF;
	}
	.s-page-wrapper{
		height: calc(100% - 44px);
		border:1px solid #fff;
	}
	.content {
		width: 85%;
		margin: 0 auto;
	}
	.loginbtn button {
		margin-top: 30upx;
		height: 77upx;
		width: 100%;
		line-height: 77upx;
		color: #ffffff;
		font-size: 30upx;
		border-radius:39upx;
		outline: 0;
		display: block;
		margin: 0;
		background:#F5A623;
	}

	button:after {
		border: 1upx solid #f2f2f2;
	}

	.logoimg {
		width: 117upx;
		height:117upx;
	}
	.sfc-logo{
		margin:120upx auto;
		.sfc-text{
			margin-top:24upx;
			color:#999999;
			font-size: 24upx;
			text-align: center;
		}
	}
	.content{
		width: 100%;
		margin-top:191upx;
		.login-text{
			color:#999999;
			font-size: 24upx;
			text-align: center;
		}
		.login-content{
			.loginBtn{
				padding:0;
				background: #fff;
				height:77upx;
				margin-top:20upx;
				.loginBtnImg{
					height:77upx;
				}
			}
			.loginBtn:after{
				border:none;
			}
			.login-text{
				margin-top:70upx;
			}
		}
	}
	
	.otherLogin{
		width: 100%;
		margin-top:191upx;
		color:#999999;
		font-size: 24upx;
		text-align: center;
	}
</style>
