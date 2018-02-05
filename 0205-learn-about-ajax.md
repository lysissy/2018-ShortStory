# 了解$.ajax()

Ajax对于Javascript来说是一个伟大的进步，它使得我们可不需通过页面跳转就能够获取后台数据，然后做进一步的操作处理，极大程度优化了用户的体验！！
但频繁的使用，对其中的参数了解甚少，一起来了解一下吧。

### 常用参数

名称 | 描述 
--------- | -------------
async | 默认是 true
beforeSend(xhr) | 默认是 true
complete(xhr,status) | 请求完成时运行的函数（在请求成功或失败之后均调用，即在 success 和 error 函数之后）
data | 规定要发送到服务器的数据
dataType | 预期的服务器响应的数据类型
error(xhr,status,error) | 如果请求失败要运行的函数
success(result,status,xhr) | 当请求成功时运行的函数
timeout | 设置本地的请求超时时间（以毫秒计)
type | 规定请求的类型（GET 或 POST）
url | 规定发送请求的 URL。默认是当前页面

### 使用方法

* 最常用的是$.get()和$.post()方法，大多数情况下不需要直接操作$.ajax()方法，除非需要操作不常用的选项。<br>
$.get()和$.post()只接收success回调函数，因为要添加error函数，小菜鸟不知道如何处理，花了很大功夫把别人写的$.get和$.post，累得吐血，殊不知jQuery中早有其它解决方案。<br>
$.get().done().fail() 是我们熟悉的链式操作，done函数中可用来操作我们需要在success回调中处理的业务，fail函数可用来操作我们需要在error回调中需要处理的业务，见参考资料3。
* 所有的选项都可以通过$.ajaxSetup()函数来全局设置。<br>
比如在Laravel框架中，用于验证授权用户和发起请求者是否是同一个人，启用了CSRF保护。这样在每次发起ajax请求之前，在$.ajaxSetup()中设置相关必要参数，核心代码如下，见参考资料2。
```javascript
$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
    }
});
```
* 常见的处理请求失败的函数error <br>
有三个参数信息：XMLHttpRequest 对象、错误信息、（可选）捕获的异常对象。<br>
如果发生了错误，错误信息（第二个参数）除了得到 null 之外，还可能是 "timeout", "error", "notmodified" 和 "parsererror"。<br>
有时候某些业务类型的判断也可以在这里处理，比如HTTP返回了401的状态码，可以通过XMLHttpRequest对象的status值来判断，继而进行相应的业务处理。
* 请求的方式type<br>
常见的当然就是"POST"和"GET"，但是还有另外的请求方式，比如"PUT"和"DELETE"，只不过只有部分浏览器支持哦～～
* $.ajax的timeout是有默认值的<br>
通常保留默认值，或者通过jQuery.ajaxSetup来全局设定，很少为特定的请求重新设置timeout。快检查看看你是不是经常多此一举呢？

    
    
###### 参考资料
1. [jQuery ajax - ajax() 方法](http://www.w3school.com.cn/jquery/ajax_ajax.asp);
2. [`[ Laravel 5.3 文档 ]` HTTP层 —— CSRF保护  &#8211;  Laravel学院](http://laravelacademy.org/post/5859.html);
3. [deferred.fail() | jQuery API中文文档(适用jQuery 1.0 - jQuery 3.1)](http://www.css88.com/jqapi-1.9/deferred.fail/)