### DatePicker 多粒度时间选择器
原文链接：[链接](http://ext.dcloud.net.cn/plugin?id=220)


可进行多粒度的时间选择器，组件名：``rattenking-dtpicker``，代码块： ruiDatePicker。

**使用方式：**

在 ``script`` 中引用组件 

```javascript
import ruiDatePicker from '@/components/rattenking-dtpicker/rattenking-dtpicker.vue';
export default {
    components: {ruiDatePicker}
}
```

在 ``template`` 中使用组件

```html
<ruiDatePicker
	fields="second"
	start="2010-00-00 00:00:00"
	end="2030-12-30 23:59:59"
	:value="value"
	@change="bindChange"
	@cancel="bindCancel"
></ruiDatePicker>
```

实际效果参考：[https://github.com/Rattenking/rui-uni-components](https://github.com/Rattenking/rui-uni-components)

**DatePicker 属性说明：**

|属性名		|类型	|默认值	                    |说明					|
|---		|----	|---	                    |---					|
|start		|String	|'1900-00-00 00:00:00'		|限制选择器选择的最小时间	|
|end		|String	|'2500-12-30 23:59:59'		|限制选择器选择的最大时间	|
|value		|String	|''	                        |当前时间选择器显示的时间	|
|fields		|String	|'second'		            |时间选择器的粒度			|
|disabled	|Boolean|false						|是否为禁用状态			|


**fields 值说明：**

|值 		|类型	|说明					|
|---		|----	|---					|
|year		|String	|选择器粒度为年			|
|month		|String	|选择器粒度为月份			|
|day		|String	|选择器粒度为天			|
|hour		|String	|选择器粒度为小时			|
|minute	    |String |选择器粒度为分钟			|
|second	    |String |选择器粒度为秒			|

**事件说明：**

|事件名称	|说明		|
|---|---|
|change	|时间选择器点击【确定】按钮时时触发的事件，参数为picker的当前的 value|
|cancel	|时间选择器点击【取消】按钮时时触发的事件|