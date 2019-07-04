<template>
	<view class="content">
		<view :class="domKey+nub+'posi same clear'"
			:animation="animationData"
			@touchstart="delStart"
			@touchmove="delMove"
			@touchend="delEnd">
			<view class="box">
				<slot v-bind:item="item"></slot>  
			</view>
			<view class="del" @click="dele(nub)">
				删除
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: ['domKey','nub','dele'],
		data() {
			return {
				left: '0',
				start: '',
				direction: true,
				animationData: {}
			}
		},
		methods: {
			delStart: function(e){				
				const vm = this;
				const query = uni.createSelectorQuery()
				query.select('.'+this.domKey+this.nub+'posi').boundingClientRect()
				vm.start = e.clientX;
				console.log('.'+this.domKey+this.nub+'posi')
				query.exec(function(res){
					console.log(res);
					vm.left = res[0].left;
				});
			},
			delMove: function(e){
				const _left = this.left;
				const dire = e.clientX - this.start;
				const left = dire + _left;
				if(dire>0){
					this.direction = true;
				} else {
					this.direction = false;
				};
				if(left >= -100){
					if(left<=0){
						this.animationFun(0, left);
					}else {
						this.animationFun(0, 0);
					}
				} else {
					this.animationFun(0, -100);
				};
			},
			delEnd: function(e){
				let _width = 0;
				this.direction ? _width = 0 : _width = -100;
				this.animationFun(1000, _width)
			},
			animationFun(dura, widht){
				let animation = uni.createAnimation({
					duration: dura,
					timingFunction: 'ease',
				});
				this.animation = animation;
				animation.translateX(widht).step();
				this.animationData = animation.export();
			}
		}
	}
</script>

<style lang="less" scoped>
	.clear:after {
	    content: '';
	    display: block;
	    clear: both;
	    width: 0;
	    height: 0;
	}
	.content {
		width: 100%;
		overflow: hidden;
		.same {
			position: relative;
			width: calc(100% + 100px);
			.box {
				float: left;
				width:  calc(100% - 100px);
				background-color: aqua;
			}
			.del {
				float: left;
				width: 100px;
				background-color: red;
			}
		}
	}
</style>
