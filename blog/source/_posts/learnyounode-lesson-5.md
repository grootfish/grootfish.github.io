---
title: learnyounode Lesson 5
tags: [node]
comments: true    // 是否开启评论
date: 2018-03-16 14:04:09
description: learnyounode 学习笔记——Lesson 5 – Filtered LS
---


### 课程目标：
```
Create a program that prints a list of files in a given directory,
  filtered by the extension of the files. You will be provided a
  directory name as the first argument to your program (e.g.
  '/path/to/dir/') and a file extension to filter by as the second
  argument.
```
### 课程思路：
* 使用`fs.readdir()`异步读取文件夹数据
* 在`readdir()`的回调函数中处理文件列表
* 使用`path.extname()`方法获取文件后缀

### My Solution:
```javascript
var fs = require('fs')
    var path = require('path')

    var folder = process.argv[2]
    var ext = '.' + process.argv[3]

    fs.readdir(folder, function (err, files) {
      if (err) return console.error(err)
      files.forEach(function (file) {
        if (path.extname(file) === ext) {
          console.log(file)
        }
      })
    })
```
### 课程总结
1. `node`异步函数采用回调函数的方法处理数据
2. `path.extname()`返回的后缀名带有`.`