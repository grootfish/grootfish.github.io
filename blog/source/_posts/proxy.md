---
title: 前端代理转发那些事
tags: [node]
comments: true    // 是否开启评论
date: 2018-09-12 16:58:19
description: proxy
---

## 代理转发的前世今生

---

> 代理对于中国人来说真的是太熟悉不过了，如果你想到外面看看，你就得需要所谓的 vpn。大多数体验过 vpn 上网的就知道代理起到什么样的作用。

## 为什么使用代理

- 分担服务器压力
- 缓存资源
- 安全
- 负载均衡
- 压缩资源文件(Gzip)
- ……

## 前端开发中使用代理

---

- 前后端联调
- 解决前端跨域问题
- ……

## 常用的 web 服务都能实现代理转发

---

- Tomcat
- Apache
- nginx
- node

## 正向代理和反向代理

---

> 正向代理代理的是客户端，而反向代理代理的是服务器端。
> 正向代理让服务端不知道谁在请求服务，而反向代理让客户端不知道服务来自哪里。

## nginx 常见配置

---

```javascript
server {
    listen       80;
    # 配置可访问域名，注意需要添加相应host配置
    server_name xxx.dev;
    #charset koi8-r;
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }

    location /api/v1 {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Real-IP $remote_addr;
        # API Server
        proxy_pass http://localhost:4000;
        proxy_next_upstream error;
    }

}
```

## node 实现代理转发

---

```javascript
var express = require("express");
var httpProxy = require("http-proxy");

var options = {
  target: "http://jsonplaceholder.typicode.com/users",
  changeOrigin: true
};
var httpProxyServer = httpProxy.createProxyServer(options);

function jsonHttpProxy(req, res) {
  httpProxyServer.web(req, res);
}
var app = express();
app.use("/users", jsonHttpProxy);
app.listen(3002);
```

## http-proxy-middleware 简单用法

---

```javascript
var express = require("express");
var proxy = require("http-proxy-middleware");

var app = express();

app.use(
  "/api",
  proxy({
    target: "http://www.example.org",
    changeOrigin: true
  })
);
app.listen(3000);
```

## 工作中配合脚手架使用

---

1. 在配置文件中`proxyTable` 代理映射
2. 使用 `http-proxy-middleware` 应用代理服务器
