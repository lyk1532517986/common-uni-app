<template>
	<view>
		<view v-for="(item,nub) in list" :key="nub">
			<leftDel
				domKey="head"
				:item="item"
				:nubDel="nub"
				:nub="nub"
				@deleClick="deleClick">
			</leftDel>
		</view>
		
		<view class="btn" @click="btn">
			点击
		</view>
	</view>
</template>

<script>
	import leftDel from './left-del.vue';
	
	export default {
		components: {
			leftDel
		},
		data() {
			return {
				list: [
					{
						id: 'a',//标识
						lastName: '啦啦1'
					},
					{
						id: '2',
						lastName: '啦啦2'
					},
					{
						id: 's',
						lastName: '啦啦3'
					},
					{
						id: 'f',
						lastName: '啦啦4'
					}
				],
			};
		},
		// 这里需要在对数据进行添加Nub
		created() {
			const vm = this;
			vm.identification();
		},
		methods: {
			deleClick(index, item){
				const vm = this;
				vm.list.splice(index, 1);
				
				
				
			},
			//注意。如果要使用左滑删除的话，循环体内必须有nub。
			identification(){
				const vm = this;
				vm.list.filter((item, nub) => {
					item.nub = item.id;
					return item;
				});
			},
			
			btn(){
				this.list.push({
					id: 66,
					lastName: '啦啦5'
				});
				//每次添加必须执行它修改一次数据
				this.identification();
			}
		}
	}
</script>

<style lang="less">

</style>
