# 浅拷贝与深拷贝

## 基础测试

+ 什么是基础数据类型？什么是引用数据类型？
+ 什么是重新赋值，什么是属性修改？
+ Array/Object的各种方法：slice，concat, assign等。

## 浅拷贝方法

1. 数组方法一： Array.prototype.slice()
	提取并返回一个新的数组，如果源数组中的元素是个对象的引用，slice拷贝的是对象的引用到新的数组。
	
2. 数组方法二： Array.prototype.concat()
	合并多个数组，并返回一个新的数组，同上，拷贝的也是对象的引用。
	
3. 对象方法一：Object.assign()
	Object.assign()拷贝的是一个或多个源对象中所有的`可枚举`的属性值，如果原对象的属性值是一个指向对象的引用，它只拷贝那个引用值。

4. 对象方法二：Object.getOwnPropertyNames拷贝不可枚举的属性
	```
	function shallowCopyOwnProperties( source ) {
		var target = {};
		var keys = Object.getOwnPropertyNames( source );

		for (var i = 0; i < keys.length; i++) {
			target[ keys[ i ] ] = source[ keys[ i ] ];
		}
		return target;
	}
	```	

5. 对象方法三： Object.getPrototypeOf 和 Object.getOwnPropertyDescriptor拷贝原型和描述符
	```
	function shallowCopy( source ) {
		var target = Object.create(Object.getPrototypeOf( source ));
		var keys = Object.getOwnPropertyNames( source );
		for( var i = 0; i < keys.length; i++) {
			Object.defineProperty(target, keys[i], Object.getOwnPropertyDescriptor( source, keys[ i ]))
		}
		return target;
	}
	```

### 浅拷贝存在的问题：

+ 只能拷贝可枚举的属性
+ 生成的拷贝对象的原型（Object）与原对象的原型不同
+ 原对象从原型继承的属性也会被拷贝到新对象中，就像是新对象的属性一样，无法区分。
+ 属性的描述符无法被复制，只读属性被拷贝之后可能是可写的。
+ 属性值是对象的话，源对象的属性会与拷贝对象的属性指向同一个对象，彼此影响。



## 深拷贝方法

> 目前还没有完美的深拷贝方案！没有完美的深拷贝方案！没有完美的深拷贝方案！

1. 方法一：借助JSON序列化手段
	``` js
	JSON.parse( JSON.stringify( object ) );
	```
	
2. 方法二：递归迭代
``` js
	function deepCopy( source ) {
		if (!source || typeof source !== "object") {
			return source;
		}
		
		var targetObj = source.constructor === Array ? [] : {};

		for (var key in source) {
			if (Object.hasOwnProperty.call(source, key)) {
				if (source[key] && typeof source[key] === "object") {
					targetObj[key] = deepCopy(source[key]);
				} else {
					targetObj[key] = source[key];
				}
			}
		}
		return targetObj;
	}
```

	**存在的问题：**	

	+ 无法实现其它引用类型Function、Date、RegExp的拷贝
	+ 对象中存在循环引用会导致调用栈溢出
	+ 通过闭包作用域来实现私有成员的这类对象不能真正的被拷贝	
3. 方法三：使用队列
	```js
	function deepClone(source) {
		if (!source || typeof source !== "object" ) {
			return;
		}

		var target = source.constructor === Array ? [] : {};
		var cloneQueue = [{
			source: source,
			target: target
		}]

		while(current = cloneQueue.shift()) {
			for (var key in current.source) {
				// current.source可能是数组，并不一定拥有hasOwnProperty方法，所以用call方法更稳妥
				// 只复制对象自身的属性，不需要那些从原型上继承的方法
				if (Object.prototype.hasOwnProperty.call(current.source, key)) {
					if(current.source[key] && typeof current.source[key] === "object") {
						current.target[key] = current.source[key].constructor === Array ? [] : {};
						cloneQueue.push({
							source: current.source[key],
							target: current.target[key]
						})
					} else {
						current.target[key] = current.source[key];
					}
				}
			}
		}
		
		return target;
	}
	```


-------

#### 拓展知识

工具函数JSON.stringify()在将JSON对象序列化为字符串时也用到了ToString。请注意，JSON字符串化并非严格意义上的强制类型转换，因为其中也涉及ToString的相关规则。

对大多数简单值来说，JSON字符串化的结果总是字符串，与toString的效果基本相同。所以所有安全的JSON值都可以使用JSON.stringify()字符串化。

安全的JSON值是指能够呈现为有效JSON格式的值。

> 什么是不安全的JSON值：undefined、function、symbol(ES6+)和包含循环引用（对象之间相互引用，形成一个无限循环）的对象，以上都不符合JSON结构标准，支持JSON的语言无法处理它们。

以上，JSON.stringify()在对象遇到undefined、function和symbol时会自动将其忽略，在数组中则会返回null（以保证单元位置不变），对包含循环引用的对象执行JSON.stringify()会出错。

-------

##### 相关拓展
不变性：Immutability

-------

#### 推荐阅读
+ [深入理解 JavaScript 对象和数组拷贝](https://juejin.im/post/5a00226b5188255695390a74)
+ [深入深入再深入 js 深拷贝对象 - 掘金](https://juejin.im/post/5ad6b72f6fb9a028d375ecf6)