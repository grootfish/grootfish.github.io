---
title: learnyounode Lesson 2
tags: [node]
date: 2018-03-14 16:41:43
description: learnyounode 学习笔记——Lesson 2-Baby Steps
---

# learnyounode Lesson 2

## 课程目标

```javascript
Write a program that accepts one or more numbers as command-line arguments
and prints the sum of those numbers to the console (stdout).

```

## 课程提示

```javascript
You can access command-line arguments via the global process object. The
  process object has an argv property which is an array containing the
  complete command-line. i.e. process.argv.

  To get started, write a program that simply contains:

     console.log(process.argv)

  Run it with node program.js and some numbers as arguments. e.g:

     $ node program.js 1 2 3

  In which case the output would be an array looking something like:

     [ 'node', '/path/to/your/program.js', '1', '2', '3' ]

  You'll need to think about how to loop through the number arguments so
  you can output just their sum. The first element of the process.argv array
  is always 'node', and the second element is always the path to your
  program.js file, so you need to start at the 3rd element (index 2), adding
  each item to the total until you reach the end of the array.

  Also be aware that all elements of process.argv are strings and you may
  need to coerce them into numbers. You can do this by prefixing the
  property with + or passing it to Number(). e.g. +process.argv[2] or
  Number(process.argv[2]).

  learnyounode will be supplying arguments to your program when you run
  learnyounode verify program.js so you don't need to supply them yourself.
  To test your program without verifying it, you can invoke it with
  learnyounode run program.js. When you use run, you are invoking the test
  environment that learnyounode sets up for each exercise.
```

## My Solution

```javascript
const arr = process.argv.slice(2);
const sum = (stdin, std) => {
  return stdin + Number(std);
};
const result = arr.reduce(sum, 0);
console.log(result);
```

### Officical Solution

```javascript
var result = 0;

for (var i = 2; i < process.argv.length; i++) result += Number(process.argv[i]);

console.log(result);
```

我在这边用到了数组的 reduce 方法,顺便去熟悉了下 reduce 的用法:

```javascript
arr.reduce(callback[, initialValue])
```

callback 有四个参数

- `accumulator` 之前函数返回结果的累加值
- `currentValue` 数组中当前值
- `currentIndex` 数组中当前值的索引(可选)
- `array` 数组本身(可选)

Baby Steps 过关!
