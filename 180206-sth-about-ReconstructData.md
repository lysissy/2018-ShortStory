## 关于表单序列化+数据重组

### 关于form表单的小知识（06.10更新）
在JavaScript中，可以利用表单字段的type属性，连同name和value属性一起实现对表单数据的序列化。浏览器提供数据的一些规则如下
1. 对表单字段的name和value进行URL编码，使用和号（&）分隔。
2. 不发送禁用的表单字段。
3. 只发送勾选的复选框和单选按钮。
4. 不发送type为‘reset’和‘button’的按钮。
5. 多选选择框中的每个选中的值单独一个条目。
6. select元素的值，就是选中的option元素的value值，如果option元素没有value特性。则是option元素的文本值。

常规实现表单序列化的代码,如下：

``` javascript
function serialize(form){
    var parts = [],
    field = null,
    i,
    len,
    j,
    optLen,
    option,
    optValue;
    
    for (i=0, len=form.elements.length; i<len; i++) {
        field = form.elements[i];
        
        switch(field.type) {
            case "select-one":
            case "select-mutiple":
                if(field.name.length) {
                    for( j=0, optLen = field.options.length; j<optLen; j++) {
                        option = field.options[j];
                        if(option.selected) {
                            optValue = "";
                            if(option.hasAttribute) {
                                optValue = (option.hasAttribute("value") ? option.value : option.text);
                            } else {
                                optValue = (option.attributes["values"].specified ? option.value : option.text);
                            }
                            parts.push(encodeURIComponent(field.name) + "=" + encodeURIComponent(optValue));
                        
                        }
                    }
                }
                break;
            case undefined:
            case "file":
            case "submit":
            case "reset":
            case "button":
                break;
            case "radio":
            case "checkbox":
                if( !field.checked) {
                    break;
                }
            default:
                if(field.name.length) {
                    parts.push(encodeURIComponent(field.name)+"="+encodeURIComponent(field.value));
                }
        }
    }
    return parts.join("&");
}
```

#### 起源
在处理存储表单数据的时候遇到了一些麻烦。页面结构：表单>表格。每单元行表单元素一致。
原本打算表单提交的时候，通过表单数据的$form.serializeArray()做一下数据重组。后来发现被disabled的表单元素，提交表单信息时被自动忽略（参考资料1）。checkbox类型的值也未被记录在表单数据中，于是萌生想试试看能不能有什么方法把不规律的字符串重组为符合规律的数组的想法。
HERE WE GO~

### 问题描述
数组中有一些无规律字符串，如["b", "a", "b", "a", "c", "b"]，如何在现有数组的基础上，通过插入数据的方法，使其按["a", "b", "c"]这样的规律排列？

### 实现思路
1. 设置一个缓存数组用来存储最后的返回结果。
2. 把原始数组中的数据处理为连续的纯数字的形式来代表各种状态，比如：0 代表 "a", 1 代表 "b" 这样。
   - 因为在Javascript中，只有数字大小的比较操作才准确。我把这转换后的数组称为`状态数组`。
3. 在循环的过程中，把无序的状态数组通过对比插入的方式调整为有序的状态数组。核心代码如下：

    ``` javascript
    for (var j = 1, count; j < status.length ){
        if(  (status[j-1]+1)%rules_len === status[j]  ){
            // 若 (pre+1)%len 和当前项相同，j指针常规递增
            j++;
        } else {
            // 若不符合要求，需补全数据，j指针不能迭代！！！
            status.splice(j, 0, (status[j-1]+1)%rules_len );
        }
    }
    ``` 
4. 根据有序的状态数组，按照对应规则，还原字符串数组。
5. 修补初始数据和末尾数据不完整的情况。

> 详细代码可查看 [表单序列化-数据重组](../2018-PracticeDemos/0206-Data-Reconstruct/index.html)

### 总结反思
1. Javascript中的枚举是什么？
2. 在讨论的过程中，有提到过`状态机`这样的概念，需要补充学习一下。
    - 拿数字状态去映射各种不同的数据，比起直接操作数据简化了许多。避免了很多数据操作上的局限性。
3. 在for循环中，经常都是i++这样的迭代，却忘记了循环本身的意义。迭代规则也可以写在{}花括号内哦～
4. 数组在Javascript中有很多神奇的用法，所以更需要深入了解其中的操作方法、迭代器、指针 各种基础知识。

#### 备注-后记
- 在实际业务中并未真正的使用这样数据重组的方法去做，而是把数据捆绑在了元素上，操作的过程中修改存储的数据信息。
- 谈起开头的那些不被FormData记录下来的数据，有人提供了readonly的属性。这样就可以被记录下数据了，有兴趣的可以试一试。


###### 参考资料
- [&lt;input type=&#34;checkbox&#34;&gt; - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input/checkbox)
- [JavaScript与有限状态机 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2013/09/finite-state_machine_for_javascript.html)

