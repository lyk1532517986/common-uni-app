<template>
	<view class="content">
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
					pageSize: 3,
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
				
				if (action === 'refresh') {	
					vm.goods_list = [];
				};
				var arr = [
					{
						goods_short_name:'商品',
						attr_str:'规格',
						act_sign:"标志"
					},{
						goods_short_name:'商品',
						attr_str:'规格',
						act_sign:"标志"
					},{
						goods_short_name:'商品',
						attr_str:'规格',
						act_sign:"标志"
					},
				];
				try {
					arr.goods_list.forEach(item=>{
						vm.goods_list.push(item);
					});
					
					if(arr.length < vm.reachBottom.pageSize){
						vm.reachBottom.isLoad=false;
					}else{
						vm.reachBottom.isLoad=true;
						vm.reachBottom.page++;
					};
				} catch(e) {
				};
				
				uni.stopPullDownRefresh();
				uni.hideLoading();
			},
		}
	}
</script>

<style>
	.clr {
		color: red;
	}
	.content {
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
