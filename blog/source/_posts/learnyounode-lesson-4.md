---
title: learnyounode-Lesson-4
tags: [node]
comments: true    // 是否开启评论
date: 2018-03-15 16:24:41
description:
---

上一节课主要讲了在node中处理同步的I/O;在node中大部分还是异步的情况。

### 课程内容:
  * fs.readFile()
  * readFile中使用callbacks来处理异步的I/O https://github.com/maxogden/art-of-node#callbacks

### 课程目标:
  ```javascript
  Write a program that uses a single asynchronous filesystem operation to
  read a file and print the number of newlines it contains to the console
  (stdout)
  ```

### My Solution:
```javascript
const fs = require('fs');
const file = process.argv[2];

fs.readFile(file,'utf8',(err,buf)=>{
  if(err){
    return console.err(err);
  }
  const result = buf.toString().split('\n').length-1;
  console.log(result);
});
```

### Officical Solution:
```javascript
var fs = require('fs')
var file = process.argv[2]

fs.readFile(file, function (err, contents) {
  // fs.readFile(file, 'utf8', callback) can also be used
  var lines = contents.toString().split('\n').length - 1
  console.log(lines)
})
```

### 课程总结

`fs.readFile(path[,options],callback)`异步I/O（和`fs.readFileSync()`相对应）
* `path <string> | <Buffer> | <URL> | <integer> filename or file descriptor`
* `options <Object> | <string>`
    * `encoding <string> | <null> Default: null`
    * `flag <string> Default: ‘r’`
* `callback <Function>`
    * `err <Error>`
    * `data <string> | <Buffer>`

