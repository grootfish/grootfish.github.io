---
title: 前端类型判断方法整理
tags: [javascript]
description: javascript 知识整理
---


 - 基本类型： String、Number、Boolean、Symbol、Undefined、Null
 - 引用类型： Object

### typeof
```javascript
	typeof '';//string 有效
	typeof 1;//number 有效
	typeof Symbol(); //symbol 有效
	typeof undefinded; //undefinded 有效
	typeof null; //object 无效
	typeof []; //object 无效
	typeof new Function(); //function 有效
	typeof new Date(); //object 无效
	typeof new RegExp(); //object 无效
```
- 对于基本类型，除`null`以外，均可以返回正确的结果
- 对于引用类型，除了 `function` 以外，一律返回object类型
- 对于 null ，返回 object 类型
- 对于 function 返回 function 类型

### instanceof
*instanceof 只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型。*
```javascript
	[] instanceof Array; //true
	{} instanceof Object; //true
	new Date() instanceof Date; //true

	function Person(){};
	new Person() instanceof Person; //true

	[] instanceof Object; //true
	new Date() instanceof Object; //true
	new Person() instanceof Object; //true
  ```
### constructor
```javascript
''.constructor == String; //true
new Number(1).constructor == Number; //true
	true.constructor == Boolean; //true
	new Function().constructor == Function; //true
	new Date().constructor == Date; //true
	new Error().constructor == Error; //true
	[].constructor == Array; //true
	document.constructor ==HTMLDocument; //true
  window.constructor == Window; //true
  ```
- null 和 undefinde 是无效的对象，没有constructor
- 函数的 constructor 是不稳定的，自定义对象，重写 prototype 后，原来的 constructor 引用丢失， constructor 会默认 Object
```javascript
	funciotn F(){}
	F.prototype = {a:'xxxx'}
	var f = new F();
	f.constructor == F; //false
	f.constructor == Object // true
```
### toString
*toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的 [[Class]] 。这是一个内部属性，其格式为 [object Xxx] ，其中 Xxx 就是对象的类型。*
```javascript
Object.toString(); //[object Object]
	Object.prototype.toString.call(''); //[object String]
	Object.prototype.toString.call(1); //[object Number]
	Object.prototype.toString.call(true); //[object Boolean]
	Object.prototype.toString.call(Symbol()); //[object Symbol]
	Object.prototype.toString.call(undefinded); // [object Undefinde]
	Object.prototype.toString.call(null); //[object Null]
	Object.prototype.toString.call(new Function()); //[object Function]
	Object.prototype.toString.call(new Date()); //[object Date]
	Object.prototype.toString.call([]); //[object Array]
	Object.prototype.toString.call(new RegExp()); //[object RegExp]
	Object.prototype.toString.call(new Error()); //[object Error]
	Object.prototype.toString.call(document); //[object HTMLDocument]
	Object.prototype.toString.call(window); //[object Window]
	```
