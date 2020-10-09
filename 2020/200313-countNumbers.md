# 浮点型运算造成精度丢失

## 产生原因

+ **Float型在内存存储的过程中造成了精度丢失**。因位数有限，存储时若截取，会造成精度丢失。
+ 在浮点型的减法运算中，**比较阶码（指数位）大小，完成对阶**；过程中，尾数右移会引起最低有效位数丢失，产生误差。

## 解决思路

整数永远可以用二进制精确表示，将小数转为整数进行运算，结果进行合理换算，得到准确答案。

## 方案解析

``` javascript
// 关于浮点数的四则运算

// 计算小数位数（包含科学计数法的数字）
function digitLength(number){
    var eSplit = number.toString().split(/[eE]/);
    var len = (eSplit[0].split('.')[1] || '').length - (+(eSplit[1] || 0));
    return len > 0 ? len : 0;
}

// 乘法
function times (num1, num2) {
    var num1Changes = Number(num1.toString().replace('.', '');
    var num2Changes = Number(num2.toString().replace('.', '');
    var baseNumber = digitLength(num1) + digitLength(num2);
    return num1Changes * num2Changes / Math.pow(10, baseNumber);
}

// 加法
function add (num1, num2) {
    var baseNum = Math.pow(10, Math.max(digitLength(num1), digitLength(num2)));
    return (times(num1, baseNum) + times(num2, baseNum)) / baseNum;
}

// 减法
function sub (num1, num2) {
    var baseNum = Math.pow(10, Math.max(digitLength(num1), digitLength(num2))));
    return (times(num1, baseNum) - times(num2, baseNum)) / baseNum;
}
 
 // test

sub(8.02-4.12) //
```

## 适用场景

## 拓展

[计算机如何实现乘法与除法运算](https://blog.csdn.net/wangjianyu0115/article/details/39639107)

如果数字中含有千分位（如：计算金额的累加值），需要如何优化处理？




