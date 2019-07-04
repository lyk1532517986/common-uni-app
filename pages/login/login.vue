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
		
		<view class="otherLogin" v-if="button_status>0">
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
			//暂无登录逻辑，强制跳转到首页
			uni.redirectTo({
				url: "/pages/index/index"
			});
		},
		onShow() {
			//登录逻辑
		},
		methods:{
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
				
				//获取下面微信授权所需参数
				// uni.setStorageSync('unionid', unionId);
				// uni.setStorageSync('userImg', avatarUrl);
				// uni.setStorageSync('userName', nickName);
				// uni.setStorageSync('openId', openId);
				vm.toDoLogin();//传入unionId
						
			},
			toDoLogin(unionid){
						if(true){
							
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
