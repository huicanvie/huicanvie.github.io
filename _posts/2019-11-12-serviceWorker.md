---
title: Service Worker 简介
author: huicanvie
layout: post
---

```text
主要介绍了service worker的几个关键生命周期，有简单的示例代码
```

## 概念

```text

1.文件离线存储，推送通知，定期后台同步等功能。
2.无法访问页面的DOM，使用postMessage接口和页面进行数据通信交互。
3.可以访问IdexedDB API,浏览器数据库
4.service Worker 的接口中使用了promise
5.在DevTools的Application下可以观察到service worker状况
6.最开始要注册，生命周期： 下载 -》 安装 -》 激活
7.使用条件：localhost在本地可以使用http，线上必须使用https，防止数据传输被篡改。

```

### 注册

``` javascript
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/sw.js').then(function(registration) {
      // Registration was successful
      console.log('ServiceWorker registration successful with scope: ', registration.scope);
    }, function(err) {
      // registration failed :(
      console.log('ServiceWorker registration failed: ', err);
    });
  });
}
// 注意：'/sw.js'路径，决定了此service worker的控制范围，
//此处是根目录，则网址/下的所有事项的fetch事件都可以监听到，
//如果是'/index/sw.js'，则只能监听到/index/
//开头的网址地址下的fetch事件。
```

### 安装

```javascript
// self 就是 service worker对象
self.addEventListener('install', function(event) {
  // 安装成功的回调
  // 可以执行缓存操作等
});
```

### 更新过程

```text
1.某个时间点访问站点页面，重新下载最新service worker js文件，如果是service Worker 文件有字节差则会视为新的service worker
2.启动，触发install事件
3.旧的service Worker仍然会运行，新的service Worker进入等待状态（waiting）
4.当前页面关闭时，旧的service Worker会被终止，新的service Worker取得控制权
5.触发activate事件，缓存管理（删除旧的缓存等）可以在此事件中完成
```

### 参考
```
摘自MDN 》》

用户首次访问service worker控制的网站或页面时，service worker会立刻被下载。

之后至少每24小时它会被下载一次。它*可能*被更频繁地下载，不过每24小时一定会被下载一次，以避免不良脚本长时间生效。

无论它与现有service worker不同（字节对比），还是第一次在页面或网站遇到service worker，如果下载的文件是新的，安装就会尝试进行。

如果这是首次启用service worker，页面会首先尝试安装，安装成功后它会被激活。

如果现有service worker已启用，新版本会在后台安装，但不会被激活，这个时序称为worker in waiting。
直到所有已加载的页面不再使用旧的service worker才会激活新的service worker。
只要页面不再依赖旧的service worker，新的service worker会被激活（成为active worker）。

你可以监听InstallEvent，事件触发时的标准行为是准备service worker用于使用，
例如使用内建的storage API来创建缓存，并且放置应用离线时所需资源。

还有一个activate事件，触发时可以清理旧缓存和旧的service worker关联的东西。

Servcie worker可以通过 FetchEvent 事件去响应请求。通过使用 FetchEvent.respondWith 方法，你可以任意修改对于这些请求的响应。
https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API
```
