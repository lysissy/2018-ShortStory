# 如何获取id选择器中带有css特性的元素？

>Because jQuery uses CSS syntax for selecting elements, some characters are interpreted as CSS notation. In order to tell jQuery to treat these characters literally rather than as CSS notation, they must be escaped by placing two backslashes in front of them.

因为jQuery使用CSS语法获取元素，某些(特殊)字符需要被转译才能作为CSS表示法。为了使jQuery能够正确识别这些字符，这些字符前面需要添加两个反斜线。

> See the Selector documentation for further details.

更多细节请查看Selector文档

```js
// Does not work:无效
$( "#some:id" )
 
// Works!有效
$( "#some\\:id" )
 
// Does not work:无效
$( "#some.id" )
 
// Works!有效
$( "#some\\.id" )
```
> The following function takes care of escaping these characters and places a "#" at the beginning of the ID string:

以下函数负责处理这些特殊字符，并且在ID字符串的开头添加了“#”字符

```js
function jq( myid ) {
 
    return "#" + myid.replace( /(:|\.|\[|\]|,|=|@)/g, "\\$1" );

}
```
> The function can be used like so:

函数的使用如下
```
$( jq( "some.id" ) )
```
