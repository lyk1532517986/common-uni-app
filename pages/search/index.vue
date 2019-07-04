<template>
	<view class="content">
		<mSearch :show="false" :placeholder="search.placeholder" :value="search.searchValue" @search="searchIpt($event)"></mSearch>
		<view class="size">
			参数说明查看：@/components/mehaotian-search/search.md
		</view>
		<view class="size">
			注意：<br/>
			1、如果点击输入后X号删除 这样 也会触发 “search” 函数
		</view>
	</view>
</template>

<script>
	import mSearch from '@/components/mehaotian-search/mehaotian-search.vue';

	export default {
		components: {
			mSearch,
		},
		data() {
			return {
				search: {
					placeholder: '商品品牌/关键字',//提示文案
					searchValue: '第一次进来的默认值',//这个可以不填、如果是从上一级页面需要待值过来。这个值才可以填
					value: '',//用户点击搜索的值
				}
			};
		},
		methods: {
			searchIpt(value) {
				const vm = this;
				vm.search.searchValue = vm.search.placeholder;//上级过来的文案进行重置为当前提示文案
				vm.search.value = value;

// 				if(!value.match(/^[ ]*$/)){
// 					//如果全部输入的空格进行拦截
// 					vm.search.value = value;
// 				} else {
// 					console.log('当前输入的全是空格value：'+value);
// 					vm.search.value = '';
// 				};
			},
		},
	}
</script>
<style scoped>
	.content {
		padding: 25upx;
	}
	.clr {
		display: inline-block;
		vertical-align: middle;
		color: red;
	}
	.size {
		font-size: 26upx;
		line-height: 37upx;
		color: #666;
	}
	
</style>
