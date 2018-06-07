# Form表单中的`submit`你不知道的事儿

> HTML中form表单元素自身具备submit()表单提交方法，在某些特殊情况下，submit方法无法正常使用。

#### 简化的原代码
```javascript
<form action="form_action.asp" method="get">
  <p>First name: <input type="text" name="fname" /></p>
  <p>Last name: <input type="text" name="lname" /></p>
  <input type="submit" value="Submit" id="submit"/>
</form>
```
#### 业务需求：
在表单提交之前，做简单的前台验证：必填项、等。前台验证通过之后提交form 表单。

#### 令人纠结的代码
```js
<script>
    document.forms[0].submit();
</script>
```
获取表单元素，使用submit方法提交的时候，页面报错
> Uncaught TypeError: document.forms[0].submit is not a function

#### 现象分析
Form表单元素无法使用submit事件，让我深深陷入恐慌之中，怀疑人生....</br>
资料1-[javascript - Submit is not a function in js - Stack Overflow](https://stackoverflow.com/questions/7684271/submit-is-not-a-function-in-js) 中提到
> Form elements get added as properties to the form object using their names, so that shadows (overrides) the form's built-in submit property (which is the function you're looking for).
> 表单元素会以它们的`name`作为属性名称添加到表单对象中，所以（如果你曾用‘submit’作为表单元素的name值）重写了表单对象原本内置的‘submit’方法（也就是覆盖了你原本想要使用的submit提交方法事件）

#### 总结
表单元素的名称也是有雷点的，需要小心避雷，类似的
``` js
<input type="button" name="submit" />
```
也都无法正常使用submit方法。
anyway，一句话总结，我们的目标是：好好学英语，命名不尴尬！


