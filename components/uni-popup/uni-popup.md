### uni-popup 弹出层
原文链接：[连接](https://ext.dcloud.net.cn/plugin?id=320)


popup 可从 top，bottom，left，right 四个方向滑出，也挺常用的


|属性名			|类型				|默认值	|说明								|
|title			|String				|		|position 为 top 时，该属性生效		|
|disable		|Boolean			|false	|超出是否禁止滚动					|
|hidden			|Boolean			|true	|遮罩是否隐藏						|
|position		|String				|right	|遮罩层的方向						|
|no-cancel		|Boolean			|false	|是否隐藏 cancel 按钮				|
|cancel-text	|String				|取消	|cancel 按钮文字						|
|confirm-text	|String				|确定	|confirm 按钮文字					|
|no-botton		|Boolean			|false	|是否隐藏 cancel 和 confirm 按钮		|
|popup-style	|String/Array/Object|		|popup 蒙版样式，可按 vue 方法写		|
|cancel-style	|String/Array/Object|		|cancel 按钮样式，可按 vue 方法写	|
|confirm-style	|String/Array/Object|		|confirm 按钮样式，可按 vue 方法写	|
|@confirm		|EventHandel		|		|点击确认触发的回调					|
|@cancel		|EventHandel		|		|点击取消触发的回调					|


position 的值
|值		|说明				|
|top	|从顶部划出，无按钮	|
|left	|从左侧划出			|
|right	|从右侧划出			|
|bottom	|从下方划出			|