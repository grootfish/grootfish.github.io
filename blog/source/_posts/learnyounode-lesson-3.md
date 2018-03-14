---
title: learnyounode Lesson 3
tags: [node]
date: 2018-03-14 16:50:14
description: learnyounode 学习笔记——Lesson 3 – My First I/O!
---

### 课程目标:

```javascript
Write a program that uses a single synchronous filesystem operation to
  read a file and print the number of newlines (\n) it contains to the
  console (stdout), similar to running cat file | wc -l.
```

### My Solution:

```javascript
const fs = require('fs');
const buffer = fs.readFileSync(process.argv[2]);

const result = buffer.toString().split('\n').length-1;
console.log(result);

 // note you can avoid the .toString() by passing 'utf8' as the
// second argument to readFileSync, then you'll get a String!
//
// fs.readFileSync(process.argv[2], 'utf8').split('\n').length - 1
```

这节课主要讲的fs的同步读取文件方法`readFileSync`,第一个参数是文件路径，第二个参数是编码格式。返回的是`node`特有的`buffer`格式，可以通过不同方法转换所需要的类型`（eg.toString()）`

好了认识了第一个node核心模块（core modules）`fs`.过关！
