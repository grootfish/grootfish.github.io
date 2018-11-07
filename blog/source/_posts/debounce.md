---
title: 函数节流和函数防抖
tags: [javascript]
comments: true    // 是否开启评论
date: 2018-11-02 14:55:25
description:
---


### 什么是throttle 和 debounce
---
> throttle 和 debounce 的功能主要是控制函数调用的频率。

- `throttle` 函数节流,将一个函数的调用频率限制在一定阈值内，例如 1s 内一个函数不能被调用两次。
- `debounce` 函数防抖,当调用函数n秒后，才会执行该动作，若在这n秒内又调用该函数则将取消前一次并重新计算执行时间，举个简单的例子，我们要根据用户输入做suggest，每当用户按下键盘的时候都可以取消前一次，并且只关心最后一次输入的时间就行了。

### throttle 和 debounce 的应用场景
---

#### throttle 应用场景
- DOM 元素的拖拽功能实现（mousemove）
- 监听滚动事件判断是否到页面底部自动加载更多

#### debounce 应用场景
- 每次 resize/scroll 触发统计事件
- 文本输入的验证（连续输入文字后发送 AJAX 请求进行验证，验证一次就好）

### 简单实现防抖 debounce
```javascript
function debounce(fn,wait){
    var timer = null;
    return function(){
        clearTimeout(timer);
        timer = setTimeout(fn,wait);
    }
}

function log(){
    console.log(1)
}

window.onscroll = debounce(log,1000);
```

### throttle 就是设置了 maxwait 的 debounce 
```javascript
function throttle(fn,wait,maxwait){
    var timer = null;
    var previous = null; //记录上一次运行时间
    return function(){
        var now = +new Date();
        previous = previous||now;
        if(now - previous>maxwait){
            clearTimeout(timer);
            fn();
            previous = now
        }else{
            clearTimeout(timer);
            timer = setTimeout(fn,wait);
        }
    }
}

function log(){
    console.log(2)
}
window.onscroll = throttle(log,500,2000)
```

### Lodash 中的防抖 debounce
---
上面的代码只是最简单的实现，存在问题和局限性。下次有机会分享一下 Lodash 中防抖的实现