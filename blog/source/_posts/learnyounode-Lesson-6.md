---
title: Learnyounode Lesson 6
tags: [node]
comments: true    // 是否开启评论
date: 2018-03-21 18:15:56
description: learnyounode 学习笔记——Lesson 6 – Make it Modular
---


### 课程目标：
和上一节课一样打印指定文件夹下面的指定后缀的文件列表，但是这节课要封装一个module去处理这个问题。module是node的核心概念。一个node程序都是由多个module组成的。

### My Solution：

#### getFiles module:
```javascript
const fs = require('fs');
const path = require('path');

const getFiles = (dir,ext,callback)=>{
    fs.readdir(dir,(err,data)=>{
        if(err)return callback(err);
        const list = data.filter(item=>{
           return path.extname(item) ==='.'+ ext;
        })
        callback(null,list)
    })
}

module.exports = getFiles;
```

#### program.js:
```javascript
const getFiles = require('./getFiles')
const dir = process.argv[2];
const extname = process.argv[3];

getFiles(dir,extname,(err,list)=>{
    if(err)return console.err(err);
    list.forEach(item=>{
        console.log(item)
    })
})
```

### 课程总结：
1. 封装`module`是`nodejs`最基本的能力。要不断巩固提升
2. `module.exports`和`exports`的区别要掌握