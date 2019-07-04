<template>
	<view id="classify">
		<view class="webview-box">
			<web-view :src="url"></web-view>
		</view>
		<view class="TakePhoto-btn" @click="TakePhoto()">
			<image src="https://wap.soufucai.com/img/XCX/sfc/static/TakePhoto-btn.png"></image>
		</view>
		
		<view class="customer-btn" @click="customerPhone()">
			<image src="https://wap.soufucai.com/img/XCX/sfc/static/Customer-btn.png"></image>
		</view>
		
	</view>
</template>

<script>
	var dateUtils = require('../../common/util.js').dateUtils;
	
	export default {
		data(){
			return {
				url:"https://wap.soufucai.com/Classifyxcx.html?index=0&version="+(new Date()).valueOf(),
				add_time:0,
				sale_user_id:'',
				mobile_phone:0,
				from:'',
				goodsId:'',
				group_recharge_id:'',
				version:(new Date()).valueOf(),
				region_id:'',
				user_id:'',
				user_token:'',
				openId:'',
				wx_openid:'',
				cat_ids:'',
				address_id:'',
				quotation_id:'',
				is_sale:"0",
				gz_user_id:''
			}
		},
		onLoad(event) {
			var vm=this;
			vm.add_time = (new Date().getTime())/1000;
			
			uni.removeStorageSync('fromPage');
			uni.removeStorageSync('type_detail');
			uni.removeStorageSync('product_id');
			
			if(event.saleUserId){
				vm.sale_user_id=event.saleUserId;
				uni.setStorageSync('saleUserId', event.saleUserId);
			}else{
				var saleUserId = uni.getStorageSync('saleUserId');
				if(saleUserId){
					vm.sale_user_id=saleUserId;
				}else{
					vm.mobile_phone=uni.getStorageSync('mobile_phone');
				}
			}
			
			vm.is_sale = uni.getStorageSync('is_sale');
			
			if(event.from){
				vm.from = event.from;
			}
			if(event.dataid){
				vm.goodsId = event.dataid;
			}
			if(event.group_recharge_id){
				vm.group_recharge_id = event.group_recharge_id;
			}
			if(event.cat_id){
				vm.cat_ids=event.cat_id;
			}
			if(event.address_id){
				vm.address_id=event.address_id;
			}
			if(event.quotation_id){
				vm.quotation_id=event.quotation_id;
			}
			if(event.userId){
				vm.gz_user_id=event.userId;
			}
			if(event.wx_openid){
				vm.wx_openid=event.wx_openid;
				uni.setStorageSync('wx_openid', event.wx_openid);
			}
			
			vm.user_id=uni.getStorageSync('user_id');
			if(vm.from==1001){
				uni.navigateTo({
					url: '/pages/index/index?from=1001&saleUserId='+vm.sale_user_id
				})
			}else if(vm.from==1002){
				uni.navigateTo({
					url: '/pages/goods-detail/goods-detail?from=1002&dataid='+vm.goodsId+'&saleUserId='+vm.sale_user_id
				})
			}else if(vm.from==1003){
				uni.navigateTo({
					url: '/pages/earn-points/earn-points?from=1003&saleUserId='+vm.sale_user_id
				})
			}else if(vm.from==3008){
				uni.navigateTo({
					url: '/pages/recharge/recharge?from=3008&saleUserId='+vm.sale_user_id
				})
			}else if(vm.from==6004){
				uni.navigateTo({
					url: '/pages/replenishment_participate/replenishment_participate?from=6004&group_recharge_id='+vm.group_recharge_id+'&saleUserId='+vm.sale_user_id
				})
			}else if(vm.from==3010){
				//判断满足时为另外一个工长
				if(vm.is_sale==0&&vm.gz_user_id!=vm.user_id){
					uni.navigateTo({
						url: '/pages/myProductionOrder/myProductionOrder'
					})
				}else{
					uni.navigateTo({
						url: '/pages/generateOrder/generateOrder?from=3010&cat_id='+vm.cat_id+'&address_id='+vm.address_id+'&quotation_id='+vm.quotation_id+'&saleUserId='+vm.sale_user_id+'&userId='+vm.gz_user_id
					})
				}
			}else if(vm.from==6003){
				uni.navigateTo({
					url: '/pages/replenishment_index/replenishment_index?from=6003&saleUserId='+vm.sale_user_id
				})
			}
			
		},
		onShow() {
			var vm=this;
			vm.version=(new Date()).valueOf();
			uni.showLoading({
				title: '努力加载中'
			});
			var region_id = '';
			var user_token = '';
			var openId = '';
			var wx_openid = '';
			var pwToLogin = '';
			region_id=uni.getStorageSync('region_id');
			user_token=uni.getStorageSync('user_token');
			openId=uni.getStorageSync('openId');
			wx_openid=uni.getStorageSync('wx_openid');
			pwToLogin=uni.getStorageSync('pwToLogin');
			
			if(region_id&&user_token&&openId){
				vm.region_id=region_id;
				vm.user_token=user_token;
				vm.openId=openId;
				if(wx_openid){
					vm.wx_openid=wx_openid;
					vm.bindWxOpenid();
				}
			}
			
			if(pwToLogin==1){
				vm.url="https://wap.soufucai.com/Classifyxcx.html?index=0&version="+vm.version+"&is_sale="+vm.is_sale+"&sale_user_id="+vm.sale_user_id+"&region_id="+vm.region_id+"&user_id="+vm.user_id+"&user_token="+vm.user_token;
				console.log(vm.url);
				vm.loginEarnpoints();
			}else{
				wx.checkSession({
					success(res) {
						vm.url="https://wap.soufucai.com/Classifyxcx.html?index=0&version="+vm.version+"&is_sale="+vm.is_sale+"&sale_user_id="+vm.sale_user_id+"&region_id="+vm.region_id+"&user_id="+vm.user_id+"&user_token="+vm.user_token;
						console.log(vm.url);
						vm.loginEarnpoints();
					},
					fail(err) {
						console.log("checkSession.fail",err);
						uni.showToast({
							title: '请先进行登录，再进行操作',
							duration: 2000,
							icon:"none",
							complete(rfs){
								if(vm.from){
									return ;
								}
								uni.setStorageSync('fromPage', '/pages/Classify/Classify');
								uni.setStorageSync('type_detail', '3001');
								uni.setStorageSync('product_id', '');
								
								uni.removeStorageSync('user_id');
								uni.removeStorageSync('unionid');
								
								setTimeout(function(){
									uni.navigateTo({
										url: "/pages/login/login"
									})
								},2000)
							}
						});
					}
				})
			}
			uni.hideLoading();
		},
		onShareAppMessage() {
			var vm=this;
			var option={
				browse_type:2,
				add_time:(new Date().getTime())/1000,
				end_time:'',
				type:3,
				type_detail:3001,
				user_id:vm.user_id,
				url:'/pages/Classify/Classify',
				product_id:'',
				title:'商品分类分享',
				region_id:vm.region_id,
				from:1,
				mobile_phone:vm.sale_user_id?vm.sale_user_id:'',
			}
			dateUtils.statisticsAdd(option);

			return {
				title: '首页',
				path: '/pages/Classify/Classify?saleUserId='+vm.mobile_phone
			}
		},
		methods: {
			bindWxOpenid(){
				var vm=this;
				uni.request({
					method:"POST",
					url: dateUtils.baseUrl+'Integral/api/bindWxOpenid',
					data: {
						user_id:vm.user_id,
						wx_openid:vm.wx_openid
					},
					dataType:"json",
					header: {'content-type': 'application/x-www-form-urlencoded'},
					success: (res) => {
						console.log(res);
						try{
							if(res.data.status>0){
								uni.removeStorageSync('wx_openid');
							}
						}catch(e){
							//TODO handle the exception
						}
						
					},
					fail: (data, code) => {
						console.log('fail' + JSON.stringify(data));
					}
				})
				
			},
			customerPhone(){
				uni.makePhoneCall({
					phoneNumber: '400-8501000'
				});
			},
			TakePhoto(){
				uni.chooseImage({
					count: 1, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album','camera'], //从相册选择
					success: function (res) {
						console.log("chooseImage",res);
						uni.previewImage({
							urls: res.tempFilePaths
						});
					}
				});
			},
			loginEarnpoints() {
				var vm=this;
				uni.request({
					url: dateUtils.baseUrl+'Integral/api/loginEarnpoints',
					data: {
						user_id:vm.user_id
					},
					success: (res) => {
						console.log(res);
						try{
							if(res.data.status>0){
								uni.showToast({
									title: res.data.msg,
									duration: 2000,
									icon:"success"
								});
							}
						}catch(e){
							//TODO handle the exception
						}
						
					},
					fail: (data, code) => {
						console.log('fail' + JSON.stringify(data));
					}
				})
				
			}
		}
	}
</script>

<style lang="less" scoped>
	page{
		height: 100%;
	}
	#classify{
		height:100%;
		.customer-btn{
			width: 114upx;
			height:114upx;
			position: fixed;
			right:0;
			top:276px;
			z-index: 112233;
			display: none;
			image{
				display: block;
				width: 114upx;
				height:114upx;
			}
		}
		.TakePhoto-btn{
			width: 114upx;
			height:114upx;
			position: fixed;
			right:0;
			top:200px;
			z-index: 112233;
			display: none;
			image{
				display: block;
				width: 114upx;
				height:114upx;
			}
		}
	}
</style>
