> 面试背景：游戏公司，技术氛围优良，待遇水平标准，主要发展海外市场。

## 一面

### 垂直水平居中的几种实现方式
1. margin:auto (宽度不足100%，盒模型水平居中)
2. 绝对定位 + marginLeft + marginTop
    (固定宽度+固定高度；思考：为什么不能用50%？)
    margin中的百分比是针对父级计算的。    
3. 绝对定位 + transform（不固定宽度+不固定高度）
4. 父级display:table； 子级display:table-cell 
    （table盒模型）
5. 父级text-align:center; 子级display:inline-block; vertical-align:middle;父级伪元素height:100%; display:inline-block; vertical-align:middle;(缺点：需要考虑inline-block间隔中的留白，如代码换行符)
6. 父级display:flex;-webkit-align-items:center;-webkit-justify-content:center;（flex盒模型）

参考： [[分享] 纯CSS完美实现垂直水平居中的6种方式 - Html5 APP 开发 - SegmentFault 思否](https://segmentfault.com/a/1190000006108996)


### 实现跨域的几种方式
1. 设置document.domain
2. 有src属性的标签（img, script, link）
3. JSONP，应用广泛，尤其是jQuery对其支持度很好（$.get, $.getJSON）
4. navigation对象（IE6\7），window.name、URL中的hash值都可以用来页面间通信。
5. 跨域资源共享CORS （Cross Origin Resource Share）
6. window.postMessage

参考： [Web开发中跨域的几种解决方案 | Harttle Land](http://harttle.land/2015/10/10/cross-origin.html)


##### delegate事件委托的原理，和bind以及on方法的区别
* on是jQuery中最基础的方法
``` javascript
delegate: function( selector, types, data, fn ) {
    return this.on( types, selector, data, fn );
}
```
参考资料：[jQuery事件：bind、delegate、on的区别 | Harttle Land](http://harttle.land/2015/06/26/jquery-event.html)

* GET请求和POST请求的区别？
参考资料：[HTTP 方法：GET 对比 POST | 菜鸟教程](http://www.runoob.com/tags/html-httpmethods.html)

* VUE实现的生命周期？
参考资料：[Vue 实例 — Vue.js](https://cn.vuejs.org/v2/guide/instance.html)

* VUE事件绑定和jquery事件绑定

* VUE中的if和show有什么区别？
不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。
参考资料：[条件渲染 — Vue.js](https://cn.vuejs.org/v2/guide/conditional.html)


##### 技术XX
* 块级元素和行间元素的区别
* JSONP的callback如何实现的？
* 什么是跨域、什么是同源？
参考： [浏览器的同源策略 - Web 安全 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)


##### 技术 二面
* TCP/IP有哪几层？HTTP属于哪一层？几次握手？Websocket有什么特别之处？
* 如何优化多个HTTP请求？（跨域、Websocket）
* 报文头部的keep-alive是什么意思？
* 对网络请求的优化处理？chunck....
* 跨域请求的处理方式？JSONP实现方式中的参数如何设置？
* 如何实现阻塞加载？
* 如何处理高品质图像的渲染？
* 如何处理大数据的网络传输？（gzip压缩等）

