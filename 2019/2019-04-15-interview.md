# 技术面试

## Vue和React的区别
+ Virtual DOM: 
    + Vue在渲染过程中，跟踪每个组件的依赖关系，不需要渲染整个组件树。
    + React每当应用的状态改变时，全部子组件都会重新渲染。（shouldComponentUpdate进行控制）
+ 模版 JSX：
    + Vue单文件，包含HTML、CSS、Javascript。
    + React中的Javascript和XML在一起组成JSX，css需要单独文件。
+ 数据
    + Vue中的data是应用数据的保存者。
    + React使用state保存应用状态，数据的更新使用setState方法。

**参考文档**：
* [Vue与React两个框架的区别和优势对比](http://caibaojian.com/vue-vs-react.html)
* [react和vue深入对比](https://github.com/hawx1993/tech-blog/issues/17)


## Vue的生命周期

beforeCreate -> created -> beforeMount -> Mounted -> beforeUpdate -> updated -> beforeDestroyed -> destroyed

#### 1. created和mounted的区别
+ created: 在实例创建完成后被立即调用。已完成的配置有：数据观测、属性和方法的运算、watch|event事件回调。$el属性不可见。
+ mounted: el被新创建的vm.$el替换，并挂载到实例上之后被调用

#### 2. 数据在哪个生命周期内处理？
+ 从后端请求得到的数据，一般都在mounted处理。（**为什么不是created？**）

## Vue的组件通信

#### 1. 父子组件通信
props+emit方法

#### 2. 跨级组件通信

#### 3. 状态管理VUEX
事件订阅、事件广播

#### 4. 双向绑定
v-model：语法糖:value和@input的简写

## Vue-Router路由相关

#### 有哪些实现方式？
hash、history

## Webpack相关
#### 1. 用过哪些插件？
vue-loader
#### 2. entry设置？
入口文件

## JS基础
#### 1. 请解释一下闭包
子函数可以使用包含函数中的变量
+ 闭包的弊端
    + 引用一直存在，内存得不到释放，占用内存。
+ 如何消除弊端？
    + 使用完毕之后，将对象设置为null，事件也？？
+ 闭包产生的原理？为什么可以产生闭包？
    + js的作用域链
    
#### 2. 请用ES5实现继承
* 原型继承、
* 构造继承、
* 组合继承、
* 寄生继承、
* 组合寄生继承

**参考资料**：
* [JavaScript继承理解](https://segmentfault.com/a/1190000015766680)
* [JavaScript实现继承的多种方式](https://juejin.im/post/5b188852e51d4506df277095)

#### 3. new操作符的工作原理
以new操作符调用构造函数会经历四个步骤：
1. 创建一个新对象
2. 讲构造函数的作用域赋给一个新对象（改变this的指向）
3. 执行构造函数中的代码
4. 判断返回值，如果没有显示返回一个对象，则使用步骤1创建的对象。

**参考资料**：[new运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)

#### 4. 实例、构造函数、原型之间的关系
每个构造函数都有一个原型对象prototype，原型对象都包含一个指向构造函数的指针constructor、实例都包含一个指向原型对象的内部指针_prototype_。

#### 5. 请解释浏览器中的事件机制，EventLoop
**参考资料**[浏览器事件循环机制](https://juejin.im/post/5afbc62151882542af04112d)

#### 6. js中的模块化CMD和AMD有了解过吗？
ES6中为import。异步模型和同步模型，require和define之类，不太清楚了。

## 混合开发
#### 1. webView的交互有什么了解的





