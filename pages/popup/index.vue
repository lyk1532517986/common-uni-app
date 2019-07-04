<template>
	<view class="content">
		<view class="size clr">
			注意：<br/>
			1、因为引入了4种方式。当点击的时候点击第2次出来的效果才是正确的效果。
		</view>
		<view class="btn" @click="popupClick('bottom')">
			浮层(bottom)
		</view>
		<view class="btn" @click="popupClick('right')">
			浮层(right)
		</view>
		<view class="btn" @click="popupClick('left')">
			浮层(left)
		</view>
		<view class="btn" @click="popupClick('top')">
			浮层(top)
		</view>
		
		
		<uni-popup :hidden='popup.hidden' :no-botton="popup.noBotton" :position='popup.position' :confirm-style='{color: "#fff", fontSize: "18px"}' @confirm="confirm" @cancel="cancel">
			<view v-for='val in 10' :key='val'>
				{{val}}、item
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniPopup from '@/components/uni-popup/uni-popup.vue';

	export default {
		components: {
			uniPopup
		},
		data() {
			return {
				popup: {
					position: 'bottom',
					noBotton: false,
					hidden: true,
				},
			};
		},
		methods: {
			popupClick(post){
				const vm = this;
				vm.popup.hidden = false;
				vm.popup.position = post;
			},
			confirm(){
				const vm = this;
				vm.popup.hidden = true;
			},
			cancel(){
				const vm = this;
				vm.popup.hidden = true;
			}
		},
	}
</script>

<style lang="less">
	.clr {
		color: red!important;
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
		padding: 0 40upx;
		height: 77upx;
		text-align: center;
		line-height: 77upx;
		background-color: #F5333D;
		color: #fff;
		margin: 50upx auto;
	}
</style>
