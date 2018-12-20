---
title: 使用 dva 开发 React 应用
tags: [javascript, React]
comments: true    // 是否开启评论
date: 2018-12-20 14:13:56
description: 使用dva开发用户管理的 CURD 应用
---

## 约定先行

- 公共组件统一放到 `components` 目录下，公共组件建议添加 `propTypes` 和 `defaultProps`
- 页面组件统一放到 `routes` 目录下
  - `services`目录定义接口调用
  - `models`目录定义 Dva 的 models
  - `views`目录处理页面渲染逻辑
  - `constants.js` 定义组件常量（可选）
  - `config.js` 定义组件配置（可选）
- 所有事件监听的方法都用 `handle` 开头。
- 把事件监听方法传给组件的时候，属性名用 `on` 开头
- 有时候 `render()` 方法里面的内容会分开到不同函数里面进行，这些函数都以 `render*` 开头
- 组件的内容编写顺序
  1. `static` 开头的类属性，如 `defaultProps`、`propTypes`。
  2. 构造函数，`constructor`。
  3. `getter/setter`方法。
  4. 组件生命周期。
  5. 组件私有方法。
  6. 事件监听方法，`handle*`。
  7. `render*`开头的方法。
  8. `render()` 方法。

## 开发步骤

### Step 1. 安装 dva-cli 并创建应用

先安装 dva-cli，并确保版本是 1.0.0-beta.2 或以上。

```bash
$ npm i dva-cli@next -g
$ dva -v
1.0.0-beta.4
```

然后创建应用：

```bash
$ dva new dashboard
$ cd dashboard
```

### Step 2. 定义路由

修改页面组件`index.js`

```javascript
module.exports = [
  {
    url: "/demands/list",
    view: "list",
    models: ["list"]
  },
  {
    url: "/demands/detail",
    view: "detail"
  }
];
```

### Step 3. 构造 service 文件

新增`src/routes/Demand/services/list.js`，内容如下

```javascript
export async function fetch(params) {
  return request("/api/demand/list", {
    method: "POST",
    data: {
      ...params
    }
  });
}
```

### Step 4. 构造 models 文件

新增 `src/routes/Demand/models/list.js`，内容如下：

```javascript
import * as demandService from "../services/list";
import { PAGE_SIZE } from "../constants";

export default {
  namespace: "demands",
  state: {
    list: [],
    pageNo: 1
  },
  reducers: {
    save(state,{payload: { data: list, pageNo }}) {
      return { ...state, list, pageNo };
    }
  },
  effects: {
    *fetch({ payload }, { call, put }) {
      const { result } = yield call(demandService.fetch, payload);
      const { data } = result;
      yield put({
        type: "save",
        payload: { data, pageNo: payload.pageNo }
      });
    }
  },
  subscriptions: {
    setup({ dispatch, history }) {
      return history.listen(({ pathname }) => {
        if (pathname === "/demands/list") {
          dispatch({
            type: "fetch",
            payload: { pageSize: PAGE_SIZE, pageNo: 1 }
          });
        }
      });
    }
  }
};
```

### Step 5. 添加界面，让列表展现出来

我们把组件存在 `src/routes/Demand/views/list.js` 里

```javascript
import React from "react";
import "../index.less";

function Demands() {
  return <div className="demands">需求列表</div>;
}

export default Demands;
```

### Step 7. 处理 loading 状态

dva 有一个管理 effects 执行的 hook，并基于此封装了 dva-loading 插件。通过这个插件，我们可以不必一遍遍地写 showLoading 和 hideLoading，当发起请求时，插件会自动设置数据里的 loading 状态为 true 或 false 。然后我们在渲染 components 时绑定并根据这个数据进行渲染。

然后在 `src/routes/Demand/views/list.js` 里绑定 loading 数据：

```javascript
+ loading: state.loading.models.users,
```

### Step 8. 处理数据 CURD

- CURD 的功能调整基本都可以按照以下三步进行：

  1. services
  2. models
  3. views

- 我们以删除功能为例

  1 . services, 修改 `src/routes/Demand/services/list.js`：

  ```javascript
  export async function remove(id) {
    return request(`/api/demand/delete/${id}`);
  }
  ```

  2 . models, 修改 `src/routes/Demand/models/list.js`：

  ```javascript
  ...
  *remove({ payload }, { call, put }) {
      yield call(demandService.remove, payload);
      yield put({ type: 'fetch', payload: { pageSize: PAGE_SIZE, pageNo: 1 } });
    },
  ...
  ```

  3 . views,修改 src/routes/Demand/views/list.js，替换 handleDelete 内容

  ```javascript
  dispatch({ type: "demands/remove", payload: id });
  ```

## 技术栈一览

![dva技术栈](https://www.dropbox.com/s/f2h92qac5cw11z1/Dva.png?dl=0)
