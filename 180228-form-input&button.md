# 表单中的提交按钮
### 分类
* input[type=submit]
* input[type=button]
* button

### 区别
+ 语义上的不同
    * button是闭合标签，元素内部可以放置除了图像映射之外的任何内容（参考资料1）
    * input是单标签。
+ 行为上的不同
    * button在IE浏览器中默认类型是"button",在其他浏览器中默认类型是"submit"，点击之后会提交表单内容。
    
### 使用细节
* button在表单中提交的数据会因浏览器的不同而异，IE提交button表单中间的内容，其他浏览器提交value属性的值。
* 在bootstrap中，由于浏览器默认样式的差异，推荐使用的是button按钮<br>（参考资料2，使用input元素设置line-height的时候，在Firefox<30的版本中会有差异）

###### 参考资料
1. [[原]&lt;button&gt;和&lt;input type=&quot;button&quot;&gt; 的区别 - 雨知 - 博客园](http://www.cnblogs.com/purediy/archive/2012/06/10/2544184.html)
2. [ CSS · Bootstrap ](https://getbootstrap.com/docs/3.3/css/#buttons)
3. [ 创建我的第一个表单 - 学习 Web 开发 | MDN ](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/Your_first_HTML_form)