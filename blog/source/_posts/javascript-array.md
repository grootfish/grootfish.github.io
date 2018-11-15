---
title: JS数组方法整理
tags: [javascript]
description: javascript 知识整理
---

# 创建一个数组

```javascript
//字面量方式
var arr = [1, 3, "9"]; //[1,3,'9']
//构造器方法
var a = Array(); //[]
var a = Array(3); //[,,]
var a = Array(1, 3, "9"); //[1,3,'9']
//ES6 Array.of()返回由所有参数值组成的数组
let a = Array.of(1, 3, "9"); //[1,3,'9']
let a = Array.of(); //[]
let a = Array.of(3); //[3]
//ES6 Array.from()将类数组对象转成真正的数组（不改变原对象，返回新数组）
//参数：
//- 第一个（必需）：要转化为真正数组的对象
//- 第二个（可选）：类似数组的map方法，对每个元素进行处理，将处理结果放入返回的新数组中
//- 第三个（可选）：用来绑定this
let obj = { 0: "a", 1: "b", 2: "c", length: 3 };
let arr = Array.from(obj); //['a','b','c']
let arr = Array.from("hello"); //['h','e','l','l','o']
let arr = Array.from(new Set(["a", "b"]));
["a", "b"];
```

## 原型方法

### 改变原数组的方法（9 个）

```javascript
let a = [1, 2, 3];
//ES5
a.splice();
a.sort();
a.pop();
a.push();
a.shift();
a.unshift();
a.revese();

//ES6
a.copyWithin();
a.fill();
```

### 不改变原数组的方法（8 个）

```javascript
let a = [1,2,3]
a.slice(begin,end);//浅拷贝数组元素
a.join();//数组转字符串
a.toLocaleString()
a.toString()
a.concat()
a.indexOf() //查找数组是否存在某个元素，返回下标
a.lastIndexOf()
//定义: 方法返回指定元素,在数组中的最后一个的索引，如果不存在则返回 -1。（从数组后面往前查找）

//ES6 扩展运算符 ... 合并数组；
let a = [2, 3, 4, 5];
let b = [ 4,...a, 4, 4];//[4,2,3,4,5,4,4]

//ES7 includes() 查找数组是否包含某个元素 返回布尔值
a.includes(searchElement,fromIndex=0);
### 遍历方法（12个）
array.forEach(function(currentValue, index, arr), thisValue);
array.every(function(currentValue, index, arr), thisValue);
array.some(function(currentValue, index, arr), thisValue);
arr.filter(function(currentValue, index, arr), thisArg);
arr.map(function(currentValue, index, arr), thisArg);
array.reduce(function(total, currentValue, currentIndex, arr), initialValue);
array.reduceRight()
//ES6
arr.find(function(currentValue, index, arr), thisArg)
arr.findIndex(function(currentValue, index, arr), thisArg)
array.keys();
array.values();
array.entries();
```
