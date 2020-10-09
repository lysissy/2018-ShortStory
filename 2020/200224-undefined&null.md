# undefined与null的区别

## 语言历史

1995年Javascript诞生时，只设置了null作为表示‘无’的值。

根据C语言的传统，null被设计成可以自动转为0。

``` javascript
Number(null) //0
Number(undefined) // NaN
```

但是，Javascript的设计者Brendan Eich,觉得这样做还不够，有两个原因：

1. null像在java里一样，被当成一个对象。但是Javascript的数据类型分为原始类型和合成类型两大类。**Brendan Eich觉得表示‘无’的值最好不是对象。**
2. Javascript的最初版本没有错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。**Brendan Eich觉得，如果null自动转为0，很不容易发现错误。**

因此，Brendan Eich又设计了一个undefined。

## 概念的区别

* null是一个表示‘无’的对象，转为数值时为0；
* undefined是一个表示‘无’的原始值，转为数值时为NaN。

## 语义的区别

#### null表示‘没有对象’，即该处不应该有值。典型用法如下：

1. 作为函数的参数，表示该函数的参数不是对象；
2. 作为对象原型链的终点。

#### undefined表示‘缺少值’，就是此处应该有一个值，但是还没有定义。典型用法如下：
    
1. 变量声明了，但没有赋值时，就等于undefined。
2. 调用函数时，应该提供的参数没有提供，该参数等于undefined。
3. 对象没有赋值的属性，该属性的值为undefined。
4. 函数没有返回值，默认返回undefined。

``` javascript
var i;
i // undefined
    
function f(x) { console.log(x) }
f() // undefined
 
var o = new Object()
o.p // undefined
 
var x = f();
x // undefined 
```

#### 参考资料
1. [undefined与null的区别](https://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)