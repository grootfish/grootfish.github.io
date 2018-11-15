---
title: learnyounode-Lesson 7
tags: [node]
comments: true    // 是否开启评论
date: 2018-03-22 17:01:16
description: learnyounode 学习笔记——Lesson 7 – HTTP Client
---

### 课程目标：

```
Write a program that performs an HTTP GET request to a URL provided to you
  as the first command-line argument. Write the String contents of each
  "data" event from the response to a new line on the console (stdout).
```

### My Solution:

```javascript
const http = require("http");
const url = process.argv[2];
http.get(url, (request, response) => {
  response.on("data", data => {
    console.log(data.toString());
  });
});
```

### Official Solution:

```javascript
const http = require("http");
const url = process.argv[2];
http
  .get(url, (request, response) => {
    response.setEncoding("utf8");
    response.on("data", console.log);
    response.on("error", console.error);
  })
  .on("error", console.error);
```

### 课程总结：

本节课主要讲的是 node 中 http 模块的使用

1. `http.get()`方法的使用。
2. `http.get()`回调函数的两个参数是`request`,`response`.都是 Node Stream object 类型。
3. Node Stream 可以用`on`方法触发 Node 事件(eg.`data`、`end`、`error`...)
4. `response`和`requeset`默认是 Node buffer 类型。可以直接调用`setEncoding`方法指定类型，也可以使用`toString()`等方法转换类型。
5. 官方解决方法中处理返回错误是最佳实践。
6. 除了`http`模块`https`模块也需要了解。
