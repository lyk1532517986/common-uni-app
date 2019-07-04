<template>
	<div class="content">
		<view class="btn" @click="btn">筛选</view>
		<view class="import">
			注意：<br/>
			1、只有多选的时候才 default 的值才可以'|'来分割默认选中那个 重置的话也是重置到默认的值.
			<view class="clr">
				（单选没有强制处理这里，如果添加多个。默认不会拦截。多个默认都会显示）
			</view>
			<br/>
			2、如果要初始默认其他值，也需要重置propertyTypeReset里面的重置后选中值。<br/>
			3、如果需要其他数据自行往getSele里面添加。<br/>
			
		</view>
		
		<uni-drawer :visible="sidePull.showPropertyType" mode="right" @close="hidePropertyType" >
			<view class="filterDrawer" v-show="sidePull.propertyTypeList.length>0">
				<view v-for="(item,index) in sidePull.propertyTypeList" :key="index">
					<view class="uni-title" >{{item.name}}</view>
					<view class="filter-list clear">
						<view class="filter-item" :class="{active:item1.active}" v-for="(item1,index1) in item.value" :key="index1" @click="propertyTypeItem(index,index1)">
							{{item1.name}}
						</view>
					</view>
				</view>
				<view class="btn-box">
					<view class="Reset-btn" @click="propertyTypeReset">
						重置
					</view>
					<view class="submit-btn" @click="propertyTypeSubmit">
						确定
					</view>
				</view>
			</view>
		</uni-drawer>
		<sideslipScreening
			:showPropertyType="true"/>
	</div>
</template>

<script>
	import uniDrawer from '../../components/uni-drawer.vue';
	import sideslipScreening from '@/components/sideslip-screening/sideslip-screening.vue';
	export default {
		components: {
			uniDrawer,
			sideslipScreening
		},
		onHide() {
			console.log('onHide隐藏侧拉111')
		},
		beforeDestroy() {
			console.log('beforeDestroy隐藏侧拉111')
		},
		destroyed() {
			console.log('destroyed隐藏侧拉111')
		},
		data(){
			return {
				sidePull: {
					moveSele: false,//是否可以多选
					default: ['0|1','','1'],//注意只有多选的时候才可以'|'来分割默认选中那个 重置的话也是重置到默认的值

					showPropertyType:false,//是否显示弹框
					propertyTypeListValue: [],//选中的值
					propertyTypeList:[
						{
							name: "第一列",
							field: "brand_id",
							value: [
								{
									cid: "177",
									name: "水",
									active: false,
								},
								{
									cid: "181",
									name: "电",
									active: false,
								},
								{
									cid: "85",
									name: "木",
									active: false,
								},
								{
									cid: "3",
									name: "瓦",
									active: false,
								},
								{
									cid: "86",
									name: "油",
									active: false,
								},
							]
						},
					],
				}
			}
		},
		methods: {
			
			btn(){
				this.showPropertyTypeFn();
			},
			propertyTypeSubmit(){
				var vm=this;
				vm.hidePropertyType();
				//propertyTypeListValue 获取当前选中的值
				vm.getSele();
				
				
				console.log( vm.sidePull.propertyTypeListValue );
			},
			hidePropertyType(){
				var vm=this;
				vm.sidePull.showPropertyType=false;
			},
			showPropertyTypeFn(){
				var vm=this;
				vm.sidePull.showPropertyType=true;
			},
			propertyTypeItem(index,index1){
				var vm = this;
				//修改当前选中的样式
				if(vm.sidePull.moveSele){
					//多选
					vm.sidePull.propertyTypeList[index].value.forEach((value,nub)=>{
						if(index1 == nub){
							value.active ? value.active = false : value.active = true;
						};
					});
				} else {
					//单选
					vm.sidePull.propertyTypeList[index].value.forEach((value,nub)=>{
						if(index1 == nub){
							value.active = true;
						} else {
							value.active = false;
						};
					});
				};
			},
			propertyTypeReset(){
				var vm=this;
				//重置默认值数据
				if(vm.sidePull.default.length > 0){
					//需要默认数据
					const def = vm.sidePull.default;
					vm.sidePull.propertyTypeList.forEach((item, nub)=>{
						if((def.length - 1) >= nub){
							const curr = def[nub].split('|');
							item.value.forEach((value,index)=>{
								value.active = curr.includes( index.toString() );
							});
							
						} else {
							console.log('default默认值有部分没选择！没选择部分已经置位已经全部置位false')
							item.value.forEach((value,index)=>{
								value.active = false;
							});
						};
					});
					
					//propertyTypeListValue 获取当前选中的值
					vm.getSele();
				}else {
					//不需要默认值。
					vm.sidePull.propertyTypeList.forEach(item=>{
						item.value.forEach((value,index)=>{
							value.active=false;
						});
					});
					vm.sidePull.propertyTypeListValue = [];
				};
			},
			getSele(){
				const vm = this;
				vm.sidePull.propertyTypeListValue=[];
				vm.sidePull.propertyTypeList.forEach(item=>{
					let _Arr = [];
					item.value.forEach((value,index)=>{
						if(value.active){
							_Arr.push(value.cid);
						};
					});
					if(_Arr.length > 0){
						let _arr = {
							name: item.field,
							value: _Arr.join('|')
						};
						vm.sidePull.propertyTypeListValue.push(_arr);
					};
				});
			},
		},
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
	.import {
		font-size: 26upx;
		line-height: 37upx;
		color: #666;
	}
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
<style lang="less" scoped>
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
