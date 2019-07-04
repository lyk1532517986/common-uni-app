<template>
	<view class="content">
		<view class="loding">下拉刷新</view>
		<view class="btn" @click="btnRegain" style="margin-bottom: 50upx;">从第一页进行加载</view>
		<view class="size" v-for="(item, nub) in goods_list" :key="nub" style="margin-bottom: 50upx;">
			<view class="clr">第{{nub + 1}}个商品</view>
			{{item.goods_short_name}} <br/>
			{{item.attr_str}} <br/>
			{{item.act_sign}} <br/>
		</view>
	</view>
</template>

<script>
	import { dateUtils } from "@/common/util.js";
	
	export default {
		data() {
			return {
				user_id: '',
				user_token: '',
				
				goods_list: [],//接口返回的数据
				reachBottom: {
					isLoad: true,//滑动到底部后是否可以继续加载
					page: 1,
					pageSize: 8,
				},
			};
		},
		
		
		
		onShow() {
			const vm = this;
			// 这里只是判断当前用户有没有登录
			vm.user_id = uni.getStorageSync('user_id');
			vm.user_token = uni.getStorageSync('user_token');
			if(!vm.user_id && !vm.user_token){
				uni.redirectTo({
					url: '../login/login',
				});
			} else {
				//上来先加载第一页
				vm.getAjax('refresh');
			};
		},
		///////////////onPullDownRefresh 是下拉刷新使用的
		onPullDownRefresh(){
			const vm = this;
			vm.getAjax('refresh');
		},
		//////////////onReachBottom是上拉加载需要的
		onReachBottom() {
			const vm = this;
			if(vm.reachBottom.isLoad){
				vm.getAjax('add');
			};
		},
		methods: {
			btnRegain(){
				const vm = this;
				//从第一页进行加载
				vm.getAjax('refresh');
			},
			
			//action == add  上拉加载更多   action == refresh 不需要上拉加载更多。从第一页进行加载
			getAjax(action = 'add'){
				const vm = this;
				uni.showLoading({
					title: '加载中'
				});

				if (action === 'refresh') {
					vm.reachBottom.page = 1;
				};
				//注意看下自己的是post还是get请求。
				//特别注意：get请求可以不添加请求头。但是post请求必须添加下面请求头。（添加原因查看uni-app官网在request方法解释的最下面）
				uni.request({
					method:"POST",
					url: dateUtils.baseUrl+'Service/Goods/searchGoodsOpen?user_id='+vm.user_id+'&user_token='+vm.user_token+'&device=wap',
					data: {
						p: vm.reachBottom.page,//当前页数  使用这个值需要和后台确认是否有效
						num: vm.reachBottom.pageSize,//每页返回几个  使用这个值需要和后台确认是否有效
						
						//下面为我示例接口使用的参数
						supplier_id: 0,
						user_id: vm.user_id,
						user_token: vm.user_token,
					},
					header: {'content-type': 'application/x-www-form-urlencoded'},
					success: (res) => {
						if (action === 'refresh') {	
							vm.goods_list = [];
						};
						try {
							//res.data.data.data.goods_list  这里需要根据实际接口返回的数据来进行修改
							res.data.data.data.goods_list.forEach(item=>{
								vm.goods_list.push(item);
							});
							
							//这里是通过res.data.data.data.goods_list 返回数据的长度来判断当前是不是到了最后一页了
							if(res.data.data.data.goods_list.length < vm.reachBottom.pageSize){
								vm.reachBottom.isLoad=false;
							}else{
								vm.reachBottom.isLoad=true;
								vm.reachBottom.page++;
							};
						} catch(e) {
							//TODO handle the exception
							console.log('数据接口错误');
						};
						
						uni.stopPullDownRefresh();
						uni.hideLoading();
					},
					fail:(err) => {
						console.log("err",err);
						uni.stopPullDownRefresh();
						uni.hideLoading();
					}
				});
				
			},
		}
	}
</script>

<style>
	.clr {
		color: red;
	}
	.loding{
		width: 100%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		position: absolute;
		left: 0;
		top: -50px;
		color: red;
	}
	.content {
		position: relative;
		padding: 25upx;
		background-color: #eee;
	}
	.size {
		font-size: 26upx;
		line-height: 37upx;
		color: #666;
	}
	.btn {
		padding: 0 20upx;
		height: 77upx;
		text-align: center;
		line-height: 77upx;
		background-color: #F5333D;
		color: #fff;
		margin: 50upx auto;
	}
	
</style>
