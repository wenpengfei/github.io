---
title: 创建一个离线优先，数据驱动的渐进式 Web 应用程序
date: 2018-01-30 14:11:11
tags:
---

![img](/images/build-an-offline-first-data-driven-pwa/2.png)

> 原文地址：[Build an offline-first, data-driven PWA](https://codelabs.developers.google.com/codelabs/workbox-indexeddb/index.html?index=..%2F..%2Findex#0)
> 译文出自：{% post_link build-an-offline-first-data-driven-pwa 夜色镇歌的个人博客 %}

# 概述

在本文中，您将学习如何使用 Workbox 和 IndexedDB 创建离线优先、数据驱动的渐进式Web应用程序（PWA）。在离线的情况下也可以使用后台同步功能将应用程序与服务器同步。

## 将会学习到

* 如何使用 Workbox 缓存应用程序
* 如何使用 IndexedDB 存储数据
* 如何在用户脱机时从 IndexedDB 中检索和显示数据
* 脱机时如何保存数据
* 如何在脱机时使用后台同步更新应用程序

## 应该了解的

* HTML, CSS, 和 JavaScript
* ES2015 Promises
* 如何使用命令行
* 熟悉一下 Workbox 
* 熟悉一下 Gulp 
* 熟悉一下 IndexedDB

## 需具备的条件

* 拥有 terminal/shell 访问权限的电脑
* Chrome 52 或更高版本
* 编辑器
* Nodejs 和 npm

# 设置

如果你没有安装 [Nodejs](https://nodejs.org/en/) 需要安装一下

之后通过下面的方式 clone 快速启动仓库

```bash
git clone https://github.com/googlecodelabs/workbox-indexeddb.git
```

或者直接下载 [压缩包](https://github.com/googlecodelabs/workbox-indexeddb/archive/master.zip)

# 安装依赖并启动服务

到下载好的 git 仓库目录中，转到 `project` 文件夹

```bash
cd workbox-indexeddb/project/
```

然后安装依赖并启动服务

```bash
npm install
npm start
```

#### 说明

这个步骤中会根据 `package.json` 定义的依赖并安装，打开 `package.json` 文件查看，有很多依赖，大部分是开发环境需要的（你可以忽略），主要的依赖是：

* [workbox-sw](https://workboxjs.org/reference-docs/latest/module-workbox-sw.html) Workbox
* [workbox-background-sync](https://workboxjs.org/reference-docs/latest/module-workbox-background-sync.html) 是 Workbox 用来后台同步的，稍后会提到
* [gulp](https://gulpjs.com/) 和 [workbox-build](https://workboxjs.org/reference-docs/latest/module-workbox-build.html) 是构建工具

`npm start` 会构建并输出到 `build` 文件夹，启动 dev server，并且会开启一个 `gulp watch` 任务。`gulp watch` 会监听文件的修改自动构建。`concurrently` 可以同时跑 `gulp` 和 dev server

## 打开应用

打开 Chrome 并且跳转到 `localhost:8081` 你会看到一个事件列表的控制台，在弹出的权限确认菜单中点击允许

![img](/images/build-an-offline-first-data-driven-pwa/1.png)

我们使用通知系统来告知用户 app 的后台同步已经更新，试着测试一下页面底部的添加功能

#### 说明

这个小项目的目标是离线保存用户的事件日历。你可以查看一下 `app/js/main.js` 文件的 `loadContentNetworkFirst ` 方法当前是怎么工作的，首先会请求 server，成功则更新页面，失败会在控制台打印一个信息，目前脱机是无法使用的，接下来我们添加一些方法使它脱机可用。

# 缓存 app shell

## 编写 service worker

要想脱机工作，就需要 server worker，现在写一个。

把下面的代码添加到 `app/src/sw.js`

```js
importScripts('workbox-sw.dev.v2.0.0.js');
importScripts('workbox-background-sync.dev.v2.0.0.js');

const workboxSW = new WorkboxSW();
workboxSW.precache([]);
```

#### 说明

在开头我们引入了 `workbox-sw` 和 `workbox-background-sync`

* `workbox-sw` 包含了 `precache` 和向 service worker 添加路由的方法
* `workbox-background-sync` 是在 service worker 中后台同步的库，稍后会提到

`precache` 方法接收一个文件列表的数组，先用一个空的，下一步我们会用 `workbox-build` 去计算出这个数组的结果。

## 构建 service worker

推荐使用 Workbox 的构建模块，比如 `workbox-build`

把下面的代码添加进 `project/gulpfile.js`

```js
gulp.task('build-sw', () => {
  return wbBuild.injectManifest({
    swSrc: 'app/src/sw.js',
    swDest: 'build/service-worker.js',
    globDirectory: 'build',
    staticFileGlobs: [
      'style/main.css',
      'index.html',
      'js/idb-promised.js',
      'js/main.js',
      'images/**/*.*',
      'manifest.json'
    ],
    templatedUrls: {
      '/': ['index.html']
    }
  }).catch((err) => {
    console.log('[ERROR] This happened: ' + err);
  });
});
```

现在取消一些注释：

gulpfile.js:
```js
// uncomment the line below:
const wbBuild = require('workbox-build');

// ...

gulp.task('default', ['clean'], cb => {
  runSequence(
    'copy',
    // uncomment the line below:
    'build-sw',
    cb
  );
});
```

保存修改，因为修改了 gulp，我们得重新跑一下，`Ctrl + C` 退出当前的进程，重新运行 `npm start`，会看到 service worker 的文件被生成在了 `build/service-worker.js`

取消 `app/index.html` 中 service worker 注册代码的注释

```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('service-worker.js')
    .then(function(registration) {
      console.log('Service Worker registration successful with scope: ',
      registration.scope);
    })
    .catch(function(err) {
      console.log('Service Worker registration failed: ', err);
    });
}
```

保存修改，刷新浏览器 service worker 就会被安装。`Ctrl + C` 关闭 dev server，再返回到浏览器中刷新页面，已经可以脱机运行了！

### 说明

在这一步中，`workbox-build` 和 `build-sw` 任务被合并到我们的 gulp 文件中，我们的构建过程是使用 `workbox-build` 库来从 `swSrc(app/src/sw.js)` 中生成 service work 到 `swDest(build/service-worker.js)`，来自 `globDirectory(build)` 的 `staticFileGlobs` 文件被注入到 `build/service-worker.js` 以供 `precache` 调用，还有每个文件的修订哈希。templatedUrls 选项告诉 Workbox 我们的站点以 index.html 的内容响应请求。

顺便贴一个 [injectManifest](https://developers.google.com/web/tools/workbox/reference-docs/latest/module-workbox-build#.injectManifest) 的链接

安装生成好的 service worker 缓存 app shell 的资源文件，Workbox 会自动去：

* 为缓存资源设置缓存优先策略，允许应用程序离线加载
* service work 更新时，使用修订哈希来更新缓存的文件

# 创建 IndexedDB 数据库

目前为止还不能离线加载数据，我们接下来创建一个 IndexDB 来保存程序的数据，数据库命名为 `dashboardr`

添加下面代码到 `app/js/main.js`

```js
function createIndexedDB() {
  if (!('indexedDB' in window)) {return null;}
  return idb.open('dashboardr', 1, function(upgradeDb) {
    if (!upgradeDb.objectStoreNames.contains('events')) {
      const eventsOS = upgradeDb.createObjectStore('events', {keyPath: 'id'});
    }
  })
}
```

取消调用 `createIndexedDB` 的注释：

```js
const dbPromise = createIndexedDB();
```

保存文件，重启 server：

```bash
npm start
```

回到浏览器刷新页面，激活 [skipWaiting](https://developers.google.com/web/ilt/pwa/tools-for-pwa-developers#chromeupdate) 并再次刷新页面，在 Chrome 中，你可以在开发者工具中的 `Application` 面板中选择 `Service Workers` 点击 `skipWaiting`，之后使用 [开发者工具](https://developers.google.com/web/ilt/pwa/tools-for-pwa-developers#indexeddb) 检查数据库是否存在。在 Chrome 中你可以在 `Application ` 面板中点击 `IndexedDB` 选择 `dashboardr` 查看 `events` 对象是否存在。

> 注意：开发者工具的 IndexedDB UI 可能不会准确的反应你数据库的情况，在 Chrome 中你可以刷新数据库查看，或者重新打开开发者工具

#### 说明

在上面的代码中，我们创建了一个 dashboardr 数据库，并把他的版本号设置为 `1` ，然后检查 events 对象是否存在，这个检查是为了避免潜在的错误，我们还给 event 提供了一个唯一的 key path `id`。

由于我们修改了 ` app/main.js` 文件，gulp 的 `watch` 任务会自动构建，Workbox 会自动更新修订哈希，然后智能更新缓存中的 `main.js`。

# 保存数据到 IndexedDB 中

现在我们保存数据到刚创建的数据库 `dashboardr` 中的 `event` 对象中。

```js
function saveEventDataLocally(events) {
  if (!('indexedDB' in window)) {return null;}
  return dbPromise.then(db => {
    const tx = db.transaction('events', 'readwrite');
    const store = tx.objectStore('events');
    return Promise.all(events.map(event => store.put(event)))
    .catch(() => {
      tx.abort();
      throw Error('Events were not added to the store');
    });
  });
}
```

然后更新 `loadContentNetworkFirst` 方法，现在这是完整的方法：

```js
function loadContentNetworkFirst() {
  getServerData()
  .then(dataFromNetwork => {
    updateUI(dataFromNetwork);
    saveEventDataLocally(dataFromNetwork)
    .then(() => {
      setLastUpdated(new Date());
      messageDataSaved();
    }).catch(err => {
      messageSaveError(); 
      console.warn(err);
    });
  }).catch(err => { // if we can't connect to the server...
    console.log('Network requests have failed, this is expected if offline');
  });
}
```

取消注释 `addAndPostEvent` 中的 `saveEventDataLocally` 调用

```js
function addAndPostEvent() {
  // ...
  saveEventDataLocally([data]);
  // ...
}
```

保存文件，刷新页面重新激活 service worker。再次刷新页面，检查一下来自网络的数据是否被保存到 `events` 中去（你可能需要刷新一下开发者工具中的 `IndexedDB`）

#### 说明

`saveEventDataLocally` 接收一个数组并一条条的保存到 IndexedDB 数据库中，我们把 `store.put` 写在了 `Promise.all` 中，这样如果某一条更新出错我们就可以终止事务。

`loadContentNetworkFirst` 方法中，一旦收到来自服务器的数据，就会更新 IndexedDB 和页面。然后，数据成功保存时，将存储时间戳，并通知用户数据可供离线使用。

在`addAndPostEvent` 中调用 `saveEventDataLocally` 方法保证了添加新的 `event` 时本地会存有最新的数据。 

# 从 IndexedDB 中获取数据

离线的时候，我们就要查询本地缓存的数据。

添加下面的代码到 `app/js/main.js` 中：

```js
function getLocalEventData() {
  if (!('indexedDB' in window)) {return null;}
  return dbPromise.then(db => {
    const tx = db.transaction('events', 'readonly');
    const store = tx.objectStore('events');
    return store.getAll();
  });
}
```

然后更新 `loadContentNetworkFirst` 方法，完整的方法如下：

```js
function loadContentNetworkFirst() {
  getServerData()
  .then(dataFromNetwork => {
    updateUI(dataFromNetwork);
    saveEventDataLocally(dataFromNetwork)
    .then(() => {
      setLastUpdated(new Date());
      messageDataSaved();
    }).catch(err => {
      messageSaveError();
      console.warn(err);
    });
  }).catch(err => {
    console.log('Network requests have failed, this is expected if offline');
    getLocalEventData()
    .then(offlineData => {
      if (!offlineData.length) {
        messageNoData();
      } else {
        messageOffline();
        updateUI(offlineData); 
      }
    });
  });
}
```

保存文件，刷新浏览器激活更新的 service worker，现在 `Ctrl + C` 关闭 dev server，返回到浏览器中刷新页面，现在 app 和数据都可以离线加载了！

#### 说明

`loadContentNetworkFirst` 被调用的时候如果没有网络连接，`getServerData` 会被 reject，之后便会进入到 `catch` 中去，然后 `getLocalEventData` 会调用本地缓存的数据。有网络连接的话会正常的请求 server 并且 `updateUI`

# 使用 workbox-background-sync

我们的 app 已经可以离线保存和浏览数据，现在我们来用 `workbox-background-sync` 把离线状态下保存的数据同步到服务端去。

把下面的的代码添加到 `app/src/sw.js`

```js
let bgQueue = new workbox.backgroundSync.QueuePlugin({
  callbacks: {
    replayDidSucceed: async(hash, res) => {
      self.registration.showNotification('Background sync demo', {
        body: 'Events have been updated!'
      });
    }
  }
});

workboxSW.router.registerRoute('/api/add',
  workboxSW.strategies.networkOnly({plugins: [bgQueue]}), 'POST'
);
```

保存，现在转到命令行：

```bash
npm run start
```

刷新浏览器，激活更新的 service worker

`Ctrl + C` 把 app 变为离线状态，添加一个 `event` 确认请求 `/api/add` 已经被添加进 `bgQueueSyncDB` 的 `QueueStore` 对象。

#### 说明

当用户试图在离线情况下添加 `event` 的时候，`workbox-background-sync` 会把失败的请求保存为一个离线队列，当用户重新联网 `backgroundSync` 会重新发送这些请求，甚至都不需要用户打开 app！但是，从联网到重新发请求的这个过程大概需要 5 分钟，下一节我们将会介绍如何在 app 中立即发送这些请求。

# 重发请求

因为重发请求会有延迟，所以用户可能回到 app 之后还没有同步数据，所以我们在用户联网的时候立即发送这些请求。

把下面的代码添加到 `app/src/sw.js`

```js
workboxSW.router.registerRoute('/api/getAll', () => {
  return bgQueue.replayRequests().then(() => {
    return fetch('/api/getAll');
  }).catch(err => {
    return err;
  });
});
```

只要用户请求服务端数据（加载或刷新页面时），该路由就会 replay 排队的请求，然后返回最新的服务端数据。这很好，但是用户还是得刷新页面去重新获取数据，我们还有更好的做法。

把下面的代码添加进 `app/js/main.js`

```js
window.addEventListener('online', () => {
  container.innerHTML = '';
  loadContentNetworkFirst();
});
```

重启 server

```bash
npm start
```

刷新浏览器激活新的 service worker，并再次刷新页面。

`Ctrl + C` 把 app 变为离线状态

添加一条 `event`

重启 server

```bash
npm start
```

这时你应该能立即收到一条数据更新的通知，检查 `server-data/events.json` 中的数据是否已经更新。

#### 说明

页面加载的时候会请求 `/api/getAll`，我们拦截了这个请求，之后主要做了两件事：

* 同步本地的离线数据
* 重新请求 `/api/getAll`

也就是在重新获取服务端的数据之前先同步

> 注意：本例中的网络请求设计的非常简单，实际情况下你可能需要考虑更多因素去减少请求的数量。

# 添加删除功能

下面的时间就交给你了，添加一个删除的功能，记得删除 IndexedDB 中的数据。

