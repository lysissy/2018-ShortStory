# [整理]-面试题目
> 一面电话面试，本次二面

## 项目技术选型从哪些方面考虑的
时间成本、产品稳定性、人员构成（开放性答案）

## js基础

#### 1. 数组的方法
* 继承方法：toString、toLocaleString、valueOf
* 转换方法：join、
* 栈和队列：push、unshift、pop、shift
* 排序方法：sort、reverse、
* 拼接方法：concat
* 操作方法：slice、splice
* 位置方法：indexOf、lastIndexOf
* 归并方法：reduce、reduceRight
* 迭代方法：map、forEach、filter、some、every

**参考资料**: [javascript中数组的22种方法](https://www.cnblogs.com/xiaohuochai/p/5682621.html)

#### 2. splice的返回值
返回一个由删除元素组成的数组，或者如果没有删除元素就返回一个空数组

#### 3. 数组转字符串的方法
* join() 方法
* toString() 方法可把数组转换为字符串，并返回结果

#### 4. 数组去重实现
```
var array = [1, 2, 1, 1, '1'];

function unique(array) {
   return Array.from(new Set(array));
}

console.log(unique(array)); // [1, 2, "1"]
```
**参考资料**: [JavaScript专题之数组去重](https://github.com/mqyqingfeng/Blog/issues/27)

#### 5. Set有了解吗？

#### 6. call和apply的区别，使用场景是什么？
1. **传参方式：**
    apply的第二项是数组或者伪数组。参数确定时使用call，参数不确定时，使用apply结合arguments

2. **使用场景：**
    * 绑定执行上下文改变this的指向
    * 借用其他对象的方法。

3. **关于bind：**
    bind()方法会创建一个新函数，称为绑定函数。bind()改变上下文环境后，并非立即执行。

#### 7. 异步和同步是什么意思？
js是单线程的，如果当前操作非常耗时，会阻塞进程，需要开启异步。
同步：函数在执行之后可以得到预期的结果。
异步：函数执行之后马上返回，但不是预期的结果，不需要等待，当得到结果之后，会主动通知。
**参考资料**[JavaScript同步和异步](https://segmentfault.com/a/1190000013039660) [JavaScript 运行机制详解(理解同步、异步和事件循环)](https://www.jianshu.com/p/561db8ff3e7a)

#### 8. 如何实现js文件的异步（非阻塞）加载？
* script标签有async（下载完成后自动执行）、defer（DOM加载之后才执行）属性，可异步加载。
* 动态创建script元素加载脚本。
* 合理放置脚本位置，body标签的底部。

## Vue相关

#### 1. mounted和created的区别？
**参考资料**：[vue 生命周期深入](https://juejin.im/entry/5aee8fbb518825671952308c)

#### 2. nextTick的使用场景？
在下次DOM更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的DOM。
具体场景：
* 修改了表单的某项值，下一步提交表单form.submit()，表单操作的步骤需要在nextTick中进行。
* 父子组件之间传递数据的时候，比如子组件验证失败时，父组件需要收集并显示错误信息，不使用nextTick可能无法及时显示。