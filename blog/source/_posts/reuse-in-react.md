---
title: React 中的代码复用
tags: [React]
comments: true    // 是否开启评论
date: 2018-12-02 14:51:27
description:
---

最近公司老项目打算使用React进行代码重构，交易模块有很多重复的逻辑，代码复用性非常重要，直接关系之后扩展业务的代码量。所以整理下在 React 中代码复用的方法。

## 纯函数（Pure Function）
在讲复用性之前，我不得不介绍一下一个函数式编程里面非常重要的概念 —— 纯函数（Pure Function）。
*一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数* 我们把纯函数的定义拆开来看：
1. 函数的返回结果只依赖于它的参数。
2. 函数执行过程里面没有副作用。

具体代码我不展开了，有兴趣可以自行Google

## 为什么要提纯函数（Pure Function）
- 因为纯函数非常“靠谱”，执行一个纯函数你不用担心它会干什么坏事，它不会产生不可预料的行为，也不会对外部产生影响。
- 纯函数的特性正视我们在构建可复用代码的时候所需要的

## 代码复用方法
### Render Props
> The term “render prop” refers to a technique for sharing code between React components using a prop whose value is a function.

使用 Render Props 来处理代码复用问题的类库包括 [React Router](https://reacttraining.com/react-router/web/api/Route) 和 [Downshift](https://github.com/paypal/downshift).

借用官网的例子：
```javascript
class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>

        {/* ...but how do we render something other than a <p>?
        <p>The current mouse position is ({this.state.x}, {this.state.y})</p> */}

        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}


class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```

### 高阶组件
> a higher-order component is a function that takes a component and returns a new component.

#### 最简单的高阶组件
```javascript
import React, { Component } from 'react'

export default (WrappedComponent) => {
  class NewComponent extends Component {
    // 可以做很多自定义逻辑
    render () {
      return <WrappedComponent />
    }
  }
  return NewComponent
}
```
现在看来好像什么用都没有，它就是简单的构建了一个新的组件类 NewComponent，然后把传进入去的 WrappedComponent 渲染出来。但是我们可以给 NewCompoent 做一些数据启动工作
```javascript
import React, { Component } from 'react'

export default (WrappedComponent, name) => {
  class NewComponent extends Component {
    constructor () {
      super()
      this.state = { data: null }
    }

    componentWillMount () {
      let data = localStorage.getItem(name)
      this.setState({ data })
    }

    render () {
      return <WrappedComponent data={this.state.data} />
    }
  }
  return NewComponent

```
#### 我们可以这样使用它
```javascript
import wrapWithLoadData from './wrapWithLoadData'

class InputWithUserName extends Component {
  render () {
    return <input value={this.props.data} />
  }
}

InputWithUserName = wrapWithLoadData(InputWithUserName, 'username')
export default InputWithUserName
```
### React hooks
- [reat hooks](https://reactjs.org/docs/hooks-intro.html)

### 总结
本来想整理以下 React 中代码复用的知识的，但写到最后发现这是一个很大的题目。光靠一篇文章根本讲不好，权当我把自己的想法写出来吧，后续得再深入研究再来分享吧。逃ing....