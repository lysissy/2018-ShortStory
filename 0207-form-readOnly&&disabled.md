# readOnly v.s. disabled 的区别
在实际业务场景中，经常有表单控件不能设置值的时候，通过设置`disabled`或者`readonly`的属性都可以实现，两者有哪些细微的区别呢？

#### 定义上的区别
1. 表面意思来看，disabled是禁用失效、readonly是只读。
2. disabled对所有的表单元素有效，readonly只对input和textarea元素有效。<br>
如果控件的 type 属性为hidden, range, color, checkbox, radio, file 或 type时，readonly属性会被忽略
#### 表现上的区别
1. 在表单传送数据时，带有disabled属性的元素不会被提交，而readonly的数据随表单数据一起提交。
2. disabled的表单元素不会获取到焦点，且用户的所有交互操作（键盘+鼠标）都无效。<br>
而readonly属性的元素仍然可以获取到焦点（可使用tab键进行导航）。


#### 关于checkbox
谈到没有被提交到的数据，还有一种情况，当input的类型为checkbox的时候，如果没有被勾选上，那么不会提交任何值（并非预想中的unchecked）<br>
另外如果没有设置checkbox类型的value值，那么提交给服务端的数据默认是字符串`"on"`哦，也不是unchecked呢～参考资料2

###### 参考资料
1. [表单中Readonly和Disabled的区别 -- 简明现代魔法](http://www.nowamagic.net/html/html_ReadonlyAndDisabled.php)
2. [&lt;input type=&#34;checkbox&#34;&gt; - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input/checkbox)