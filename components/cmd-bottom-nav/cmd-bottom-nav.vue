<template>
  <view class="cmd-bottom-nav" :class="[textAuto ? 'cmd-bottom-nav-text-auto':'',fixed ? 'cmd-bottom-nav-fixed':'']"
    :style="setColorStyle">
    <view :class="['cmd-bottom-nav-box',select == index ? 'cmd-bottom-nav-active':'']" :style="select == index ? setActiveFontColorStyle : ''"
      @tap="$_click(index)" v-for="(nav,index) in listNav" :key="index">
      <view class="cmd-bottom-nav-box-icon">
        <!-- <cmd-icon v-if="nav.icon && !nav.src" :type="nav.icon" size="24" :color="select == index ?activeFontColor :  fontColor"></cmd-icon> -->
      </view>
      <image v-if="nav.src && !nav.icon" class="cmd-bottom-nav-box-img" :src="select == index ? nav.srcSelect:nav.src"
        mode="aspectFit"></image>
      <text class="cmd-bottom-nav-box-text">{{nav.text}}</text>
    </view>
  </view>
</template>

<script>
  // import cmdIcon from "../cmd-icon/cmd-icon.vue"

  export default {
    name: 'cmd-bottom-nav',
//     components: {
//       cmdIcon
//     },
    props: {
      /**
       * 导航列表选中项
       */
      current: {
        type: Number,
        default: 0
      },
      /**
       * 导航列表
       */
      list: {
        type: Array,
        default: () => {
          return [];
        }
      },
      /**
       * 文字颜色
       */
      fontColor: {
        type: String,
        default: ''
      },
      /**
       * 底部上边线颜色
       */
      borderColor: {
        type: String,
        default: ''
      },
      /**
       * 背景颜色
       */
      backgroundColor: {
        type: String,
        default: ''
      },
      /**
       * 激活文字颜色
       */
      activeFontColor: {
        type: String,
        default: ''
      },
      // 只在激活状态显示文本
      textAuto: {
        type: Boolean,
        default: false
      },
      // 固定到页面底部
      fixed: {
        type: Boolean,
        default: true
      }
    },

    data() {
			
      return {
				listNav: [
					{
						"pagePath": "/pages/index/index",
						"text": "组件", 
						"src": "../../static/home.png",
						"srcSelect": "../../static/homeactive.png"
					},
					{
						"pagePath": "/pages/template/template",
						"text": "模板",
						"src": "../../static/business.png",
						"srcSelect": "../../static/businessactive.png"
					}
				],
        // 选中项
        select: this.current,
      };
    },

    computed: {
      /**
       * 底部导航栏颜色样式
       */
      setColorStyle() {
        let colorStyle = '';
        // 文字颜色
        if (this.fontColor != '') {
          colorStyle += `color:${this.fontColor};`;
        }
        // 上边线颜色
        if (this.borderColor != '') {
          colorStyle += `border-top: 1px ${this.borderColor} solid;`;
        }
        // 背景颜色
        if (this.backgroundColor != '') {
          colorStyle += `background: ${this.backgroundColor};`;
        }
        return colorStyle;
      },
      /**
       * 激活文字样式
       */
      setActiveFontColorStyle() {
        let activeFontColorStyle = '';
        if (this.activeFontColor != '') {
          activeFontColorStyle += `color:${this.activeFontColor};`;
        }
        return activeFontColorStyle;
      }
    },
		onShow() {
			const vm = this;
			// 这里需要判断用户是否可以显示这个导航
// 			const userInfo = JSON.parse( uni.getStorageSync('userInfo') );
// 			if(userInfo){
// 				console.log('有登录状态')
// 				//这里判断是否显示用户推荐标签
// 				if(Number( userInfo.access_recommend_user) == 1) {
// 					vm.listNav.push({
// 						"pagePath": "/pages/recommendation/recommendation",
// 						"text": "用户推荐",
// 						"src": "../../static/recommendation.png",
// 						"srcSelect": "../../static/recommendationactive.png"
// 					});
// 				};
// 				
// 			} else {
// 				console.log('无登录状态需要重新登录');
// // 				uni.navigateTo({
// // 					url: '/pages/login/login'
// // 				})
// 			};
		},
    methods: {
      /**
       * 点击事件
       */
      $_click(index) {
        this.select = index;
				if(this.current != index){
					uni.redirectTo({
						url: this.listNav[index].pagePath
					})
				}
      }
    }

  }
</script>

<style lang="scss">
  /* 底部导航栏样式变量 */
  $cmd-bottom-nav-color : #000;
  $cmd-bottom-nav-color-active: #000;
  $cmd-bottom-nav-background : #fff;
  $cmd-bottom-nav-border-color : #dadada;

  .cmd-bottom-nav {
    position: relative;
    overflow: hidden;
    white-space: nowrap;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    height: 100upx;
    padding: 0;
    margin: 0 auto;
    color: $cmd-bottom-nav-color;
    background: $cmd-bottom-nav-background;
    border-top: 3upx $cmd-bottom-nav-border-color solid;

    // 导航项内每项
    &-box {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      font-size: 28upx;
      line-height: 1;
      text-align: center;
      text-overflow: ellipsis;
      overflow: hidden;
      user-select: none;
      opacity: 1;
      min-width: 64upx;
      height: 100%;
      background: transparent;
      transition: all .2s cubic-bezier(.4, 0, .2, 1);

      &-img {
        width: 44upx;
        height: 44upx;
        opacity: 1;
      }

      &-icon {
        opacity: 1;
      }

      &-text {
        margin-top: 8upx;
        display: block;
        transition: all .2s cubic-bezier(.4, 0, .2, 1);
      }

      // 激活状态的导航项
      &.cmd-bottom-nav-active {
        color: $cmd-bottom-nav-color-active;
        opacity: 1;

        .cmd-bottom-nav-box {
          &-img {
            opacity: 1;
          }

          &-icon {
            opacity: 1;
          }
        }
      }
    }
  }

  /* 只在激活时显示文本 */
  .cmd-bottom-nav-text-auto {
    .cmd-bottom-nav-box {
      padding-left: 0;
      padding-right: 0;
      min-width: 68upx;

      &-img {
        padding-top: 0;
        transition: all .2s cubic-bezier(.4, 0, .2, 1);
      }

      &-icon {
        padding-top: 34upx;
        transition: all .2s cubic-bezier(.4, 0, .2, 1);
      }

      &-text {
        transform: scale(0);
      }

      &.cmd-bottom-nav-active {
        padding-left: 38upx;
        padding-right: 38upx;

        .cmd-bottom-nav-box {
          &-img {
            padding-top: 0;
            opacity: 1;
          }

          &-icon {
            padding-top: 0;
            opacity: 1;
          }

          &-text {
            transform: scale(1);
          }
        }

      }
    }
  }

  /* 固定到页面底部 */
  .cmd-bottom-nav-fixed {
    &.cmd-bottom-nav {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      z-index: 100;
      margin-bottom: -3upx;
    }
  }
</style>
