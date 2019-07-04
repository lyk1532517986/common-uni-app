<template>
	<view class="s-page-wrapper is-100vh">
		<view class="sfc-logo">
			<view class="is-flex is-column is-justify-center  is-align-center is-height-100">
				<image src="https://wap.soufucai.com/img/XCX/sfc/static/logo.png" mode="aspectFit" class="logoimg"></image>
			</view>
		</view>
		<view class="content">
			<view class="has-mglr-10 ">
				<view class=" has-mgb-20 ">
					<input type="number" maxlength="11" v-model="login.phone" placeholder="请输入手机号" class="is-input1 " @input="BindInput" data-val="phone" placeholder-style="color:#666"/>
				</view>
				<view class=" has-radius has-mgb-10 codebox">
					<input type="number" v-model="login.code" maxlength="6" placeholder="请输入验证码" class="is-input1 " placeholder-style="color:#666" @input="BindInput" />
					<view class="codeimg" @tap="getsmscode">{{smsbtn.text}}</view>
				</view>
				<view class="has-mgl-15 sfc-user-protocol-box">
					<navigator url="#" class=" has-radius is-grey link-box" hover-class="">
						<text>点击登录表示您已阅读并同意</text><text class="sfc-user-protocol">《搜辅材用户协议》</text>
					</navigator>
				</view>
				<view class=" loginbtn has-radius has-mgt-15">
					<button :loading="login.loading" @tap="defaultHandlerLogin"> {{ login.loading ? "登录中...":"登 录"}} </button>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				login: {
					loading: false,
					phone:"",
					code:""
				},
				smsbtn: {
					text: '获取验证码',
					status: false,
					codeTime: 60
				},
				timerId: null,
			};
		},
		methods:{
			defaultHandlerLogin:function(){
				this.login.loading = true;
				setTimeout((e=>{
					this.login.loading = false;
				}),1500);
				console.log(JSON.stringify(this.login)); 
			},
			BindInput:function(e){
				var dataval = e.currentTarget.dataset.val;
				this.login[dataval] = e.detail.value; 
			},
			getsmscode: function() {
				if(!this.timerId){
					this.timerId = setInterval(() => {
						var codeTime = this.smsbtn.codeTime;
						codeTime--;
						this.smsbtn.codeTime = codeTime;
						this.smsbtn.text = codeTime + "S";
						if (codeTime < 1) {
							clearInterval(this.timerId);
							this.smsbtn.text = "重新获取";
							this.smsbtn.codeTime = 60;
							this.smsbtn.status = false;
						}
					},
					1000);
				}
				return false;
			}
		}
	}
</script>

<style lang="less" scoped>
	@import "../../common/simplepro.css";
	
	page {
		min-height: 100%;
		background-color: #FFFFFF;
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
	}
	.sfc-user-protocol-box{
		.link-box{
			font-size:12upx;
			.sfc-user-protocol{
				color:#F5A623;
			}
		}
	}
	
	.is-input1 {
		height: 77upx;
		width: 100%;
		line-height: 77upx;
		color: #353535;
		font-size: 30upx;
		box-sizing: border-box;
		appearance: none;
		border: 1upx solid #ddd;
		box-shadow: none;
		border-radius:39upx;
		outline: 0;
		display: block;
		padding-left: 30upx;
		margin: 0;
		font-family: inherit;
		background: #fff;
		resize: none;
	}
	.codebox{
		position: relative;
		.codeimg {
			position: absolute;
			font-size: 30rpx;
			top:1px;
			right:30upx;
			z-index: 999;
			width: 153rpx;
			text-align: center;
			color: #999999;
			background: #fff;
			border-top-right-radius: 37rpx;
			border-bottom-right-radius: 37rpx;
			height: 74rpx;
			line-height:74rpx;
		}
	}
</style>
