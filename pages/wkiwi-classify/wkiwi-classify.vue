<template>
	<view id="classif" >
		<view class="uni-tab-bar">
			<view id="header">
				<view class="nvbt" style="width: 24%;" @click="toSelectCity">
					<view class="nvtt" style="width: 100%;font-size: 15px;">
						<image src="../../static/location2.png" class="locate-img"></image>
						<view id="current_region_name" >{{region_name}}</view>
					</view>
				</view>
				<view class="nvtt" style="color: #B3B3B3 !important; width: 72%;" @click="searchToClick">
					<view id="title_search">
						<view class="search">
							<image src="../../static/search-icon.png" class="search-icon"></image>
							商品品牌/关键字
						</view>
					</view>
				</view>
				<view class="nvbt" style="width: 14%;" v-if="false">
					<image src="../../static/message.png" class="message-img"></image>
				</view>
			</view>
			<scroll-view id="tab-bar" class="uni-swiper-tab" scroll-x>
				<view v-for="(tab,index) in tabBars" :key="tab.id" :class="['swiper-tab-list',tabIndex==index ? 'active' : '']" :id="tab.id"
				 @click="tapTab(index)">{{tab.name}}</view>
			</scroll-view>
			<view class="nav">
				<scroll-view class="nav-left" scroll-y >
					<view class="nav-left-item" @click="categoryClickMain(index)" :key="index" :class="index==categoryActive?'active':''"
						v-for="(item,index) in classifyData">
						{{item.cat_name}}
					</view>
				</scroll-view>
				<view class="nav-right">
					<scroll-view class="swiper-brand-list uni-swiper-tab" scroll-x>
						<view v-for="(brand,index) in brandArr" :key="index" :class="['swiper-brand-item',brandIndex==index ? 'active' : '']"
						 @click="brandClick(index)">{{brand.brand_name}}</view>
					</scroll-view>
					<scroll-view class="sfc-goods-list" scroll-y @scrolltolower="loadMore" @scroll="isScroll" :scroll-top="initScroll">
						<view class="goodsInfo" v-if="productList.length>0">
							<view class="goods-item" v-for='(item,index) in productList' :key="index" @click="navPageToClick(item)">
								<view class="infoBox myborder clear" >
									<view class="infoLeft">
										<view class="imgBox">
											<image :src="item.goods_thumb" mode="widthFix" class="goodsImg"></image>
										</view>
									</view>
									<view class="infoCenter">
										<view class="sfcp">{{item.goods_name}}</view>
										<view class="sfcp1">
											<text style="color:#F23D00;">¥ {{item.goods_price>0?item.goods_price:'***'}}</text>
											<view class="fucaibi" v-if="item.virtual_money>0">
												赠{{item.virtual_money}}辅材币
											</view>
										</view>
									</view>
								</view>
							</view>
						</view>
						<view class="order-none" v-else>
							<image src="../../static/goods-none.png" alt=""></image>
							<view>暂无此分类信息</view>
						</view>
					</scroll-view>
				</view>
			</view>
		</view>
		
		<RedEnvelope :show="showRedEnvelope" @close="closeRedEnvelope" :title="RedEnvelopetitle" @click="openRedEnvelope"></RedEnvelope>
		<RedEnvelopeopen :show="showRedEnvelopeopen" @close="closeRedEnvelopeopen" :issueType="issue_type" :price="RedEnvelopeopenprice" :title="RedEnvelopeopentitle" @click="RedEnvelopeopenGoUrl"></RedEnvelopeopen>
		<Replenishment :show="showReplenishment" @close="closeReplenishment" @click="openReplenishment"></Replenishment>
		<ShareGuest :show="showShareGuest" @close="closeShareGuest" @click="openShareGuest" :bonusTypeList="bonus_type_list" :shareGuestImg='shareGuestImg' :shareGuestBtn='shareGuestBtn'></ShareGuest>
	</view>
</template>

<script>
	var dateUtils = require('../../common/util.js').dateUtils;
	import RedEnvelope from "@/components/RedEnvelope.vue"
	import RedEnvelopeopen from "@/components/RedEnvelopeopen.vue"
	import Replenishment from "@/components/replenishment.vue"
	import ShareGuest from "@/components/shareGuest.vue"
	
	export default {
		components: {
			RedEnvelope,RedEnvelopeopen,Replenishment,ShareGuest
		},	
		data() {
			return {
				initScroll:0,
				categoryActive: 0,
				classifyData:[],
				tabIndex: 0,
				tabBars: [
					{
						name: '水',
						id: 'shui',
						type:0,
						cat_id:1
					}, {
						name: '电',
						id: 'dian',
						type:1,
						cat_id:2
					},{
						name: '木',
						id: 'mu',
						type:1,
						cat_id:3
					},{
						name: '瓦',
						id: 'wa',
						type:1,
						cat_id:4
					},{
						name: '油',
						id: 'you',
						type:1,
						cat_id:5
					},{
						name: '五金',
						id: 'wujin',
						type:1,
						cat_id:6
					}
				],
				brandIndex:0,
				brandArr:[],
				redEnvelopeId:'',
				issue_type:'',
				getActivityType:1,
				sign:'',
				user_id:'',
				user_token:'',
				region_id:'',
				openId:'',
				wx_openid:'',
				isLoad:true,
				page:1,
				pageSize:10,
				productList:[],
				sale_user_id:'',
				mobile_phone:0,
				from:'',
				goodsId:'',
				group_recharge_id:'',
				cat_ids:'',
				address_id:'',
				quotation_id:'',
				is_sale:"0",
				gz_user_id:'',
				key:"8ccac150d7a42ff670bf921469d9c6fb",
				region_name:'',
				showReplenishment:false,
				showShareGuest:false,
				bonus_type_list:[],
				shareGuestImg:'https://wap.soufucai.com/img/shareGuestImg1.png',
				shareGuestBtn:'确定'
			}
		},
		onLoad(event) {
			var vm=this;
			
			uni.removeStorageSync('fromPage');
			uni.removeStorageSync('type_detail');
			uni.removeStorageSync('product_id');
			
			if(event.saleUserId){
				vm.sale_user_id=event.saleUserId;
				uni.setStorageSync('saleUserId', event.saleUserId);
			}else{
				var vm=this;
				var saleUserId = '';
				saleUserId = uni.getStorageSync('saleUserId');
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
			
			vm.user_id = uni.getStorageSync('user_id');
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
			uni.showLoading({
				title: '努力加载中'
			});
			vm.region_id = uni.getStorageSync('region_id');
			vm.region_name = uni.getStorageSync('region_name');
			var user_token=uni.getStorageSync('user_token');
			var openId=uni.getStorageSync('openId');
			var pwToLogin=uni.getStorageSync('pwToLogin');
			if(user_token){
				vm.user_token=user_token;
				vm.openId=openId;
				vm.secondCategorys();
				var wx_openid = '';
				wx_openid = uni.getStorageSync('wx_openid');
				if(wx_openid){
					vm.wx_openid=wx_openid;
					vm.bindWxOpenid();
				}
				if(vm.is_sale!=-1){
					vm.getActivity();
					if(vm.sale_user_id){
						vm.getBonus(2);
					}else{
						vm.checkBonus();
					}
				}
			}
			if(pwToLogin==1){
				vm.loginEarnpoints();
			}else{
				wx.checkSession({
					success(res) {
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
		watch:{
			page(val){
				var vm=this;
				if(val==1){
					vm.initScrollFn();
				}
			}
		},
		methods: {
			searchToClick(){
				uni.navigateTo({
					url: '/pages/search/index',
				});
			},
			navPageToClick(item){
				console.log(item)
				uni.navigateTo({
					url: '/pages/searchCommodityDetails/index?goods_id='+item.goods_id+'&product_id='+item.product_id,
				});
			},
			checkBonus(){
				var vm=this;
				uni.request({
					url:dateUtils.baseUrl+"Integral/api/newUserPackage?user_id="+vm.user_id+"&user_token="+vm.user_token,
					data:{
						user_id:vm.user_id,
						region_id:vm.region_id,
					},
					success(res){
						console.log(res);
						if(res.data.status>0){
							try{
								if(res.data.can_receive==0){
									vm.showShareGuest=false;
								}else{
									vm.showShareGuest=true;
									vm.shareGuestImg = "https://wap.soufucai.com/img/shareGuestImg0.png";
									vm.shareGuestBtn = "立即领取";
									vm.bonus_type_list=res.data.bonus_type_list;
								}
							}catch(e){
								//TODO handle the exception
							}
							
						}
					}
				})
			},
			getBonus(type){
				var vm=this;
				uni.request({
					url:dateUtils.baseUrl+"Integral/api/getBonus?user_id="+vm.user_id+"&user_token="+vm.user_token,
					data:{
						user_id:vm.user_id,
						region_id:vm.region_id,
						sale_user_id:vm.sale_user_id?vm.sale_user_id:''
					},
					success(res){
						console.log(res);
						if(type==2){
							if(res.data.status>0){
								try{
									if(res.data.data.length>0){
										vm.bonus_type_list=res.data.bonus_type_list;
									}
									if(res.receive_status>0){
										vm.showShareGuest=true;
										vm.shareGuestImg = "https://wap.soufucai.com/img/shareGuestImg1.png";
										vm.shareGuestBtn = "确定";
									}else{
										vm.showShareGuest=false;
									}
								}catch(e){
									//TODO handle the exception
								}
								
							}
						}else{
							vm.showShareGuest=false;
							uni.showToast({
								title:'已成功领取新人大礼包',
								duration:2000,
								icon:'none'
							})
						}
						
					}
				})
			},
			closeShareGuest(){
				this.showShareGuest=false;
			},
			openShareGuest(){
				if(this.shareGuestBtn == "立即领取"){
					this.getBonus(1);
				}else{
					this.showShareGuest=false;
				}
			},
			closeReplenishment(){
				this.showReplenishment=false;
			},
			openReplenishment(){
				this.showReplenishment=false;
				uni.navigateTo({
					url: '/pages/replenishment_index/replenishment_index'
				});
			},
			secondCategorys(){
				var vm=this;
				uni.request({
					method:'POST',
					url:dateUtils.baseUrl+"Service/Goods/secondCategorys",
					data:{
						cat_id:vm.tabBars[vm.tabIndex].cat_id,
						region_id: vm.region_id,
						supplier_id:'',
						device:"xcx",
					},
					dataType:"json",
					header: {'content-type': 'application/x-www-form-urlencoded'},
					success(res){
						console.log(res);
						if(res.data.data.code==705){
							vm.classifyData=res.data.data.data;
							console.log(vm.classifyData);
							vm.getBrands();
						}
					}
				})
			},
			getBrands(){
				var vm=this;
				uni.request({
					method:'POST',
					url:dateUtils.baseUrl+"Service/Goods/brands",
					data:{
						cat_id: vm.classifyData[vm.categoryActive].cat_id,
						region_id: vm.region_id,
						supplier_id:"",
						device:"xcx"
					},
					dataType:"json",
					header: {'content-type': 'application/x-www-form-urlencoded'},
					success(res){
						console.log(res);
						if(res.data.data.code==725){
							try{
								vm.brandArr=[];
								vm.brandArr.push({
										brand_id:'0',
										brand_name:'全部',
										selected:false
									})
								res.data.data.data.brands.forEach((item,index)=>{
									vm.brandArr.push(item);
								})
							}catch(e){
								//TODO handle the exception
							}
							vm.isLoad=true;
							vm.loadMore('refresh');
						}
					}
				})
			},
			loadMore(action='add') {
				var vm=this;
				
				if(!vm.isLoad){
					uni.stopPullDownRefresh();
					return ;
				}
				
				if (action === 'refresh') {
					vm.page=1;
				}
				if(vm.productList.length<1){
					uni.showLoading({
						title: '努力加载中'
					});
				}
				
				uni.request({
					method:'POST',
					url: dateUtils.baseUrl+"Service/Goods/cateBrandGoodsSku?user_id="+vm.user_id+"&user_token="+vm.user_token+"&device=xcx",
					data:{
						cat_id:vm.classifyData[vm.categoryActive].cat_id,
						brand:vm.brandArr[vm.brandIndex].brand_id,
						region_id:vm.region_id,
						supplier_id:0,
						user_id:vm.user_id,
						num:vm.pageSize,
						page:vm.page,
						sort:'',
						order:''
					},
					dataType:"json",
					header: {'content-type': 'application/x-www-form-urlencoded'},
					success: (res) => {
						console.log(res.data.data);
						try{
							if (action === 'refresh') {
								vm.productList = [];
							}
							if(res.data.data.code==707){
								
								if(res.data.data.data.length>0){
									res.data.data.data.forEach(item => {
										item.show=false;
										item.loaded=false;
										vm.productList.push(item);
									});
									
									if (action === 'refresh') {
										setTimeout(function(){
											vm.initScrollFn();
										},1000)
									}
									if(res.data.data.data.length<vm.pageSize){
										vm.isLoad=false;
									}else{
										vm.isLoad=true;
										vm.page++;
									}
								}
							}
						}
						catch(err){
						}
						uni.stopPullDownRefresh();
						uni.hideLoading();
					},
					fail:(err) => {
						console.log("err",err);
						console.log('fail' + JSON.stringify(data));
						uni.stopPullDownRefresh();
						uni.hideLoading();
					}
				});
			},
			toSelectCity(){
				uni.navigateTo({
					url: '/pages/selectCity/selectCity'
				});
			},
			initScrollFn(){
				this.initScroll=0;
			},
			isScroll(event){
				if(this.initScroll==0){
					this.initScroll=event.detail.scrollTop;
				}
			},
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
				
			},
			getActivity(){
				var vm=this;
				uni.request({
					method:'POST',
					url:dateUtils.baseUrl+"Service/RedEnvelope/getActivity?user_token="+vm.user_token+"&user_id="+vm.user_id,
					data:{
						user_id:vm.user_id,
						region_id:vm.region_id,
						type:vm.getActivityType,
					},
					dataType:"json",
					header: {'content-type': 'application/x-www-form-urlencoded'},
					success(res){
						console.log(res);
						var resdata=res.data.data.data;
						var code=res.data.data.code;
						vm.sign=resdata.sign;
						//暂无红包 1030001
						if(code==1){
							//有红包，显示红包组件
							vm.redEnvelopeId=resdata.id;
							vm.showRedEnvelope=true;
							vm.RedEnvelopetitle=resdata.title;
						}else{
							vm.getActivityList();
						}
					}
				})
				
			},
			getActivityList(){
				var vm=this;
				uni.request({
					url:dateUtils.baseUrl+"Integral/api/getActivityList",
					data:{
						user_id:vm.user_id,
						region_id:vm.region_id,
						user_token:vm.user_token,
					},
					success(res){
						console.log(res);
						if(res.data.status>0){
							var no_participate=res.data.no_participate;
							if(no_participate.length>0){
								no_participate.forEach(item=>{
									if(item.activity_type==5){
										vm.showReplenishment=false;
									}
								})
							}else{
								var showReplenishmentB = uni.getStorageSync("showReplenushment");
								if(showReplenishmentB){
									
								}else{
									vm.showReplenishment=true;
								}
							}
						}
					}
				})
				
			},
			openRedEnvelope(){
				var vm=this;
				uni.request({
					method:'POST',
					url:dateUtils.baseUrl+"Service/RedEnvelope/receive?user_token="+vm.user_token+"&user_id="+vm.user_id,
					data:{
						user_id:vm.user_id,
						region_id:vm.region_id,
						type:vm.getActivityType,
						red_envelope_id:vm.redEnvelopeId,
						order_id:vm.orderInfo.order_id,
						source:'xcx',
						sign:vm.sign
					},
					dataType:"json",
					header: {'content-type': 'application/x-www-form-urlencoded'},
					success(res){
						var resdata=res.data.data.data;
						var code=res.data.data.code;
						//暂无红包 1030001
						vm.showRedEnvelope=false;
						vm.showRedEnvelopeopen=true;
						if(code==1){
							vm.RedEnvelopeopenprice=resdata.amount;
							vm.issue_type=resdata.issue_type;
							if(resdata.issue_type==1){
								vm.RedEnvelopeopentitle="红包已存入您的余额";
							}else if(resdata.issue_type==2){
								vm.RedEnvelopeopentitle="辅材币已存入您的账户";
							}
						}
					}
				})
			},
			closeRedEnvelope(){
				var vm=this;
				vm.showRedEnvelope=false;
			},
			closeRedEnvelopeopen(){
				var vm=this;
				vm.showRedEnvelopeopen=false;
			},
			RedEnvelopeopenGoUrl(){
				var vm=this;
				vm.showRedEnvelopeopen=false;
				uni.switchTab({
					url:'/pages/me/me'
				})
			},
			async categoryClickMain(index) {
				if(this.categoryActive == index){
					return false;
				}else{
					this.categoryActive = index;
					this.brandIndex = 0;
					this.getBrands();
				}
			},
			async tapTab(index) {
				if (this.tabIndex === index) {
					return false;
				} else {
					this.tabIndex = index;
					this.categoryActive = 0;
					this.brandIndex = 0;
					this.secondCategorys();
				}
			},
			async brandClick(index){
				this.brandIndex = index;
				this.isLoad=true;
				this.loadMore('refresh');
			}
		}
	}
</script>

<style lang="less" scoped>
	.ellipsis() {
	    overflow: hidden;
	    text-overflow: ellipsis;
	    white-space: nowrap;
	}
	.clear:after {
	    content: '';
	    display: block;
	    clear: both;
	    width: 0;
	    height: 0;
	}
	.ellipsis2() {
	    overflow:hidden; 
		text-overflow:ellipsis;
		display:-webkit-box; 
		-webkit-box-orient:vertical;
		-webkit-line-clamp:2; 
	}
	.myborder:before {
		content: ' ';
		position: absolute;
		pointer-events: none;
		box-sizing: border-box;
		left: 30upx;
		right: 0;
		bottom: 0;
		-webkit-transform: scaleY(0.5);
		transform: scaleY(0.5);
		border-bottom: 1px solid #ddd;
	}
	#classif {
		display: flex;
		background: #FBFBFB;
		overflow: hidden;
		height:100%;
		box-sizing: border-box;
		.uni-tab-bar{
			#header{
				background: #fff;
				width: 100%;
				height: 96upx;
				box-sizing: border-box;
				font-size: 40upx;
				text-align: center;
				line-height: 96upx;
				.nvbt {
					width: 24%;
					height: 96upx;
					line-height: 96upx;
					float: left;
					color: white;
					font-size: 32upx;
				}
				.nvtt {
					width: 70%;
					height: 96upx;
					line-height: 96upx;
					color: #FFFFFF;
					float: left;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
					font-size: 34upx;
				}
				.locate-img {
					width: 34upx;
					height: 40upx;
					display: block;
					float: left;
					margin: 24upx -24upx 0 20upx;
				}
				.message-img{
					width: 44upx;
					height: 44upx;
					vertical-align:middle;
				}
				#current_region_name{
					float: left;
					width: 128upx;
					height: 96upx;
					line-height: 96upx;
					color: #666;
					padding-left: 20upx;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
				}
				#title_search{
					padding:20upx 0;
					box-sizing: border-box;
					.search{
						width:100%;
						height:56upx;
						line-height:56upx;
						background:rgba(204,204,204,0.3);
						border-radius:100upx;
						text-align: left;
						box-sizing: border-box;
						padding:0 30upx;
						.search-icon{
							width:28upx;
							height:28upx;
							vertical-align:middle;
							margin-right:12upx;
						}
					}
				}
			}
			#tab-bar{
				height:78upx;
				line-height:78upx;
				border-bottom: 1px solid #ddd;
				background: #fff;
				.swiper-tab-list {
					width: 10%;
					margin:0 3.333%;
					box-sizing: border-box;
					height:78upx;
					line-height: 78upx;
					display: inline-block;
					font-size: 26upx;
				}
				.active{
					color:#F5A623;
					border-bottom: 2px solid #F5A623;
				}
			}
			
			.nav {
				display: flex;
				width: 100%;
				height:calc(100% - 100upx);
				margin-top:20upx;
				justify-content: space-between;
				.nav-left {
					width: 25%;
					height:100%;
					background: #fff;
					.nav-left-item {
						height: 80upx;
						line-height: 80upx;
						font-size: 28upx;
						display: flex;
						align-items: center;
						justify-content: center;
					}
					.active {
						color: #F5A623;
						background: #fff;
						border-right: 0;
					}
				}
				.nav-right {
					width: calc(75% - 20upx);
					height:100%;
					background: #fff;
					.swiper-brand-list{
						height:93upx;
						line-height: 53upx;
						box-sizing: border-box;
						padding:20upx;
						border-bottom: 1px solid #ddd;
						.swiper-brand-item{
							display: inline-block;
							min-width: 76upx;
							box-sizing: border-box;
							text-align: center;
							height:48upx;
							font-size:24upx;
							font-weight:500;
							color:#333333;
							line-height:33upx;
							margin-right:20upx;
							padding:9upx 15upx;
							background: #F5F5F5;
							border-radius:4upx;
						}
						.swiper-brand-item:last-child{
							margin-right:0upx;
						}
						.active{
							background:#F5A623;
							color:#fff;
						}
					}
					.sfc-goods-list{
						height:calc(100% - 93upx);
						.order-none{
							width: 100%;
							padding-top:50%;
							text-align: center;
							image{
								width: 220upx;
								height:206upx;
							}
							view{
								color:#333333;
								font-size: 24upx;
								margin-top:40upx;
							}
						}
						.goodsInfo{
							.goods-item{
								background: #fff;
								.infoBox{
									position: relative;
									padding:20upx 25upx;
									padding-top:23upx;
									box-sizing: border-box;
									.infoLeft{
										width:137upx;
										height:137upx;
										line-height:137upx;
										float:left;
										.imgBox{
											width:137upx;
											height:137upx;
											line-height:137upx;
											margin:0 auto;
											text-align: center;
											overflow: hidden;
											background: #999;
											position: relative;
											.goodsImg{
												max-width: 100%;
												max-height:100%;
												display:block;
											}
											.cuxiao{
												width: 70upx;
												height:70upx;
												position:absolute;
												left:0;
												bottom:0;
											}
											.hot{
												width: 85upx;
												height:31upx;
												position:absolute;
												left:0;
												top:0;
											}
										}
									}
									.infoCenter{
										padding-left:157upx;
										box-sizing: border-box;
										position: relative;
										.sfcp{
											width: 100%;
											height:80upx;
											line-height:40upx;
											font-size:28upx;
											color: #333333;
											text-align: left;
											font-weight: 500;
											.ellipsis2;
										}
										.sfcp1{
											margin-top:15upx;
											height:42upx;
											line-height:42upx;
											font-size:30upx;
											color: #333333;
											text-align: left;
											font-weight: 600;
											.ellipsis;
										}
										.fucaibi{
											padding:0 10upx;
											height:36upx;
											background:rgba(147,147,147,1) linear-gradient(126deg,rgba(255,180,27,1) 0%,rgba(252,194,74,1) 100%);
											border-radius:4upx;
											box-sizing: border-box;
											font-size:22upx;
											color:#fff;
											font-weight:500;
											display: inline-block;
											margin-left:30upx;
										}
									}
								}
								.infoBox:before{
									left:0;
								}
							}
							
						}
					}
				}
			}
		}
	}
	
</style>
