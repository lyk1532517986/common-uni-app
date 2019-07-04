### mehaotian-search 搜索框
原文链接：[链接](https://ext.dcloud.net.cn/plugin?id=94)



|属性名			|类型		|默认值				|说明																																									|
|mode			|number		|1					|对齐方式 ,可选值: 1 [默认]文字左对齐 ，2 文字居中对齐																													|
|button			|string		|outside			|按钮位置，可选值: 'outside' [默认]按钮在搜索框外 'inside' 按钮在搜索框内																								|
|show			|boolean	|true				|按钮显示规则 ，可选值: true [默认]如果mode为1 ，则为始终显示按钮，mode为2，则获取焦点显示按钮。 false 如果mode为1 ，则为获取焦点显示按钮，mode为2，则为输入时显示按钮	|
|placeholder	|string		|请输入搜索内容		|占位提示 ，最多7个字符 ，否者可能会显示不全																															|
|backgroundColor|string		|#fff				|背景色																																									|
|border			|string		|1px #f5f5f5 solid	|输入框线条样式																																							|
|value			|string		|''					|从上一级页面携带过来的值。可以不填
|search			|function	|-					|点击搜索按钮，返回输入框内输入的值

