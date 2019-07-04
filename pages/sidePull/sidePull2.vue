<template>
	<view>
		<view class="btn" @click="btn">筛选</view>
		<view class="import">
			注意：<br/>
			1、如果要初始默认其他值，也需要重置（filterReset）里面的重置后选中值。
		</view>
		<uni-drawer :visible="sidePull.rightDrawerVisible" :mode="sidePull.mode" @close="closeRightDrawer" >
			<view class="filterDrawer">
				<view class="uni-title" v-if="sidePull.rightDrawerList.length>0">商品分类</view>
				<view class="filter-list clear" v-if="sidePull.rightDrawerList.length>0">
					<view class="filter-item" :class="{active:item.active}" v-for="(item,index) in sidePull.rightDrawerList" :key="index" @click="filterDrawerItem('order_status', sidePull.rightDrawerList, item.type, index)">
						{{item.name}}
					</view>
				</view>
				
				<view class="uni-title" v-if="sidePull.rightDrawerList1.length>0">二级分类</view>
				<view class="filter-list clear" v-if="sidePull.rightDrawerList1.length>0">
					<view class="filter-item" :class="{active:item.active}" v-for="(item,index) in sidePull.rightDrawerList1" :key="index" @click="filterDrawerItem('time_type', sidePull.rightDrawerList1, item.type, index)">
						{{item.name}}
					</view>
				</view>
				
				<view class="uni-title" v-if="sidePull.rightDrawerList2.length>0">三级分类</view>
				<view class="filter-list clear" v-if="sidePull.rightDrawerList2.length>0">
					<view class="filter-item" :class="{active:item.active}" v-for="(item,index) in sidePull.rightDrawerList2" :key="index" @click="filterDrawerItem('level_three', sidePull.rightDrawerList2, item.type, index)">
						{{item.name}}
					</view>
				</view>
				
				<view class="btn-box">
					<view class="Reset-btn" @click="filterReset">
						重置
					</view>
					<view class="submit-btn" @click="filterSubmit">
						确定
					</view>
				</view>
			</view>
		</uni-drawer>
	</view>
</template>

<script>
	import uniDrawer from '../../components/uni-drawer.vue';
	
	export default {
		components: {
			uniDrawer,
		},
		data() {
			return {
				sidePull: {
					mode: 'right',//侧拉方向 (目前只有right和left)
					rightDrawerVisible: false,//是否显示当前右侧框
					//注意：如果要初始默认其他值，也需要重置（filterReset）里面的重置后选中值。
					rightDrawerListValue: {type:'',name:'全部1',active:true},//选中值
					rightDrawerList:[
						{
							type:'',
							name:'全部1',
							active:true
						},
						{
							type:'1',
							name:'电工',
							active: false
						},
						{
							type:'2',
							name:'木工',
							active: false
						},
						{
							type:'3',
							name:'瓦工',
							active: false
						},
						{
							type:'4',
							name:'油工',
							active: false
						},
						{
							type:'5',
							name:'水工',
							active: false
						}
					],
					
					rightDrawerListValue1: {type:'',name:'全部2',active:true},
					rightDrawerList1: [
						{
							type:'',
							name:'全部2',
							active:true
						},
						{
							type:'1',
							name:'PPR',
							active:false
						},
						{
							type:'2',
							name:'PVC',
							active:false
						},
						{
							type:'3',
							name:'PVC',
							active:false
						}
					],
					
					rightDrawerListValue2: {type:'',name:'全部3',active:true},
					rightDrawerList2:[
						{
							type:'',
							name:'全部3',
							active:true
						},
						{
							type:'1',
							name:'PPR',
							active:false
						},
						{
							type:'2',
							name:'PVC',
							active:false
						},
						{
							type:'3',
							name:'PVC',
							active:false
						}
					],
				},
				
			};
		},
		methods: {
			btn(){
				this.showRightDrawer();
			},

			filterSubmit(){
				var vm = this;
				vm.closeRightDrawer();//关闭浮层
				console.log('选中的值');
				console.log(vm.sidePull.rightDrawerListValue);
				console.log(vm.sidePull.rightDrawerListValue1);
				console.log(vm.sidePull.rightDrawerListValue2);
				
			},
			closeRightDrawer() {
				this.sidePull.rightDrawerVisible = false;
			},
			showRightDrawer() {
				this.sidePull.rightDrawerVisible = true;
			},
			filterDrawerItem(filterType,list,type,index){
				var vm=this;
				switch (filterType){//赋值当前选中了那个
					case 'order_status':
						vm.sidePull.rightDrawerListValue = list[index];
						break;
					case 'time_type':
						vm.sidePull.rightDrawerListValue1 = list[index];
						break;
					case 'level_three':
						vm.sidePull.rightDrawerListValue2 = list[index];
						break;
					default:
						console.log('目前：'+filterType+' 没有配置')
						break;
				};
				
				
				//修改当前选中的样式。
				list.forEach(function(item, index){
					item.active=false;
				});
				list[index].active=true;
			},
			filterReset(){
				var vm=this;
				//重置选项
				if(vm.sidePull.rightDrawerList.length>0){
					vm.sidePull.rightDrawerList.forEach(function(item, index){
						item.active=false;
					});
					vm.sidePull.rightDrawerList[0].active=true;
				};
				if(vm.sidePull.rightDrawerList1.length>0){
					vm.sidePull.rightDrawerList1.forEach(function(item, index){
						item.active=false;
					});
					vm.sidePull.rightDrawerList1[0].active=true;
				};
				if(vm.sidePull.rightDrawerList2.length>0){
					vm.sidePull.rightDrawerList2.forEach(function(item, index){
						item.active=false;
					});
					vm.sidePull.rightDrawerList2[0].active=true;
				};
				
				//清空之前选中的值 默认为第一个
				vm.sidePull.rightDrawerListValue = vm.sidePull.rightDrawerList[0];
				vm.sidePull.rightDrawerListValue1 = vm.sidePull.rightDrawerList1[0];
				vm.sidePull.rightDrawerListValue2 = vm.sidePull.rightDrawerList2[0];
			},
			
		}
	}
</script>
<style>
	.btn {
		width: 236upx;
		height: 77upx;
		text-align: center;
		line-height: 77upx;
		background-color: #F5333D;
		color: #fff;
		margin: 50upx auto;
	}
</style>
<!-- 下面为侧拉样式 -->
<style lang="less">
	.clear:after {
	    content: '';
	    display: block;
	    clear: both;
	    width: 0;
	    height: 0;
	}
	
	.filterDrawer{
		padding:30upx;
		position: relative;
		height:100%;
		box-sizing: border-box;
		.uni-title{
			height:37upx;
			font-size:26upx;
			font-weight:600;
			color:rgba(51,51,51,1);
			line-height:37upx;
			padding:0;
			margin-bottom: 20upx;
		}
		.filter-list{
			.filter-item{
				width: 108upx;
				height: 57upx;
				line-height: 57upx;
				text-align: center;
				background:rgba(245,245,245,1);
				border-radius:4upx;
				font-size: 24upx;
				margin-right:19upx;
				margin-bottom:19upx;
				float:left;
			}
			.filter-item:nth-child(4n){
				margin-right:0upx;
			}
			.filter-item.active{
				color: #fff;
				background-color: #F5333D;
			}
		}
		.btn-box{
			display: flex;
			position: absolute;
			left:30upx;
			right:30upx;
			bottom: 30upx;
			.Reset-btn{
				width:236upx;
				height:77upx;
				line-height:77upx;
				border-radius:4upx;
				border:1px solid #F5333D;
				color:#F5333D;
				text-align: center;
				box-sizing: border-box;
				margin-right:25upx;
			}
			.submit-btn{
				width:236upx;
				height:77upx;
				line-height:77upx;
				background:#F5333D;
				color:#fff;
				border-radius:4upx;
				text-align: center;
				box-sizing: border-box;
			}
		}
	}
</style>
