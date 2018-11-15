---
title: lodash 的 debounce 实现
tags: [javascript,lodash]
comments: true    // 是否开启评论
date: 2018-11-07 16:46:30
description:
---

# 防抖 debounce

> 防抖的原理就是：你尽管触发事件，但是我一定在事件触发 n 秒后才执行，如果你在一个事件触发的 n 秒内又触发了这个事件，那我就以新的事件的时间为准，n 秒后才执行，总之，就是要等你触发完事件 n 秒内不再触发事件，我才执行，真是任性呐!

## 简单代码实现

---

```javascript
function debounce(fn, wait) {
  var timer = null;
  return function() {
    clearTimeout(timer);
    timer = setTimeout(fn, wait);
  };
}
```

---

这只是简单的实现函数防抖，存在很多问题和局限性。这次来分析一下 lodash 中的 debounce 实现。

## 存在问题

---

在看 lodash 源码之前先来分析一下简单实现存在的问题

### this 指向问题

---

上面的代码 debounce 返回的函数 this 指向了 Window 对象。在实际需求中我们希望 this 指向正确的对象（一般为触发事件的 DOM 元素）。

```javascript
function debounce(func, wait) {
  var timeout;

  return function() {
    var context = this;

    clearTimeout(timeout);
    timeout = setTimeout(function() {
      func.apply(context);
    }, wait);
  };
}
```

### event 对象

---

我们使用 debounce 方法包装之后，我们会丢失原来事件绑定的 event 对象，我们需要把他找回来。

```javascript
function debounce(func, wait) {
  var timeout;
  return function() {
    var context = this;
    var args = arguments;

    clearTimeout(timeout);
    timeout = setTimeout(function() {
      func.apply(context, args);
    }, wait);
  };
}
```

### 立即执行问题

---

上面的版本解决了 this 指向和 event 对象的问题，但是在实际使用中我们会有一个很常见的需求，我不希望非要等到事件停止触发后才执行，我希望立刻执行函数，然后等到停止触发 n 秒后，才可以重新触发执行。我们再来改造原来的代码：

```javascript
function debounce(func, wait, leading) {
  var timeout;

  return function() {
    var context = this;
    var args = arguments;

    if (timeout) clearTimeout(timeout);
    if (leading) {
      // 如果已经执行过，不再执行
      var callNow = !timeout;
      timeout = setTimeout(function() {
        timeout = null;
      }, wait);
      if (callNow) func.apply(context, args);
    } else {
      timeout = setTimeout(function() {
        func.apply(context, args);
      }, wait);
    }
  };
}
```

## lodash 的 debounce 实现

上面的代码已经比较完善了，下面来看下 lodash 中的代码实现

```javascript
function debounce(func, wait, options) {
  let lastArgs, lastThis, maxWait, result, timerId, lastCallTime;

  // 参数初始化
  let lastInvokeTime = 0; // func 上一次执行的时间
  let leading = false;
  let maxing = false;
  let trailing = true;

  // 基本的类型判断和处理
  if (typeof func != "function") {
    throw new TypeError("Expected a function");
  }
  wait = +wait || 0;
  if (isObject(options)) {
    // 对配置的一些初始化
  }

  function invokeFunc(time) {
    const args = lastArgs;
    const thisArg = lastThis;

    lastArgs = lastThis = undefined;
    lastInvokeTime = time;
    result = func.apply(thisArg, args);
    return result;
  }

  function leadingEdge(time) {
    // Reset any `maxWait` timer.
    lastInvokeTime = time;
    // 为 trailing edge 触发函数调用设定定时器
    timerId = setTimeout(timerExpired, wait);
    // leading = true 执行函数
    return leading ? invokeFunc(time) : result;
  }

  function remainingWait(time) {
    const timeSinceLastCall = time - lastCallTime; // 距离上次debounced函数被调用的时间
    const timeSinceLastInvoke = time - lastInvokeTime; // 距离上次函数被执行的时间
    const timeWaiting = wait - timeSinceLastCall; // 用 wait 减去 timeSinceLastCall 计算出下一次trailing的位置

    // 两种情况
    // 有maxing:比较出下一次maxing和下一次trailing的最小值，作为下一次函数要执行的时间
    // 无maxing：在下一次trailing时执行 timerExpired
    return maxing
      ? Math.min(timeWaiting, maxWait - timeSinceLastInvoke)
      : timeWaiting;
  }

  // 根据时间判断 func 能否被执行
  function shouldInvoke(time) {
    const timeSinceLastCall = time - lastCallTime;
    const timeSinceLastInvoke = time - lastInvokeTime;

    // 几种满足条件的情况
    return (
      lastCallTime === undefined || //首次
      timeSinceLastCall >= wait || // 距离上次被调用已经超过 wait
      timeSinceLastCall < 0 || //系统时间倒退
      (maxing && timeSinceLastInvoke >= maxWait)
    ); //超过最大等待时间
  }

  function timerExpired() {
    const time = Date.now();
    // 在 trailing edge 且时间符合条件时，调用 trailingEdge函数，否则重启定时器
    if (shouldInvoke(time)) {
      return trailingEdge(time);
    }
    // 重启定时器，保证下一次时延的末尾触发
    timerId = setTimeout(timerExpired, remainingWait(time));
  }

  function trailingEdge(time) {
    timerId = undefined;

    // 有lastArgs才执行，意味着只有 func 已经被 debounced 过一次以后才会在 trailing edge 执行
    if (trailing && lastArgs) {
      return invokeFunc(time);
    }
    // 每次 trailingEdge 都会清除 lastArgs 和 lastThis，目的是避免最后一次函数被执行了两次
    // 举个例子：最后一次函数执行的时候，可能恰巧是前一次的 trailing edge，函数被调用，而这个函数又需要在自己时延的 trailing edge 触发，导致触发多次
    lastArgs = lastThis = undefined;
    return result;
  }

  function cancel() {}

  function flush() {}

  function pending() {}

  function debounced(...args) {
    const time = Date.now();
    const isInvoking = shouldInvoke(time); //是否满足时间条件

    lastArgs = args;
    lastThis = this;
    lastCallTime = time; //函数被调用的时间

    if (isInvoking) {
      if (timerId === undefined) {
        // 无timerId的情况有两种：1.首次调用 2.trailingEdge执行过函数
        return leadingEdge(lastCallTime);
      }
      if (maxing) {
        // Handle invocations in a tight loop.
        timerId = setTimeout(timerExpired, wait);
        return invokeFunc(lastCallTime);
      }
    }
    // 负责一种case：trailing 为 true 的情况下，在前一个 wait 的 trailingEdge 已经执行了函数；
    // 而这次函数被调用时 shouldInvoke 不满足条件，因此要设置定时器，在本次的 trailingEdge 保证函数被执行
    if (timerId === undefined) {
      timerId = setTimeout(timerExpired, wait);
    }
    return result;
  }
  debounced.cancel = cancel;
  debounced.flush = flush;
  debounced.pending = pending;
  return debounced;
}
```

## lodash 中的优化

1. `maxWait` 参数保证超过一定时间保证调用一次函数
2. `trailing` 参数保证延迟结束后调用一次函数
3. 加了取消(cancel)、刷新(flush)、暂停(pending) 防抖的方法。
4. 兼容了被包装函数有返回值的情况

具体使用可以查看[官方中文文档](http://lodash.net/docs/4.16.1.html#_debouncefunc-wait0-options)
