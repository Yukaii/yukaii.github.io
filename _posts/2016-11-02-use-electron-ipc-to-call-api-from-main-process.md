---
layout: post
title: "在 Electron 使用 IPC 串聯前端和 Node API"
---

這是我最近在實作噗浪 electron app - [**Puraku**][puraku] 時，使用的抽象化寫法。

先談一下背景。其實在官方的 [Plurk API][plurk-api] 頁面上就已經有 [JavaScript 的噗浪 API Library][purakujs]了，不過它沒有包成 npm 可以直接使用，而且還相依於 [node-oauth][node-oauth] 套件，看名字就知道和 Node 有關。

這次寫的 [puraku][puraku] 是一個以前端為主的桌面軟體，所以我勢必要對噗浪的 API 套件做些改寫。

**更新**： 其實 Electron renderer process 就能呼叫 node API 了，只是我先入為主的以為 main process 才能使用，所以引發了這個軟體問題 XDrz。

## 重新封裝 API Library

我已經包裝成 [purakujs][purakujs]，它是個可以直接使用的 Plurk API Node library，雖然這年頭也沒多少工程師在串噗浪 API 了 😅 。值得一提的是 yarn 對 `npm link` 的支援不太好，在設定本機開發環境跑完 `npm link` 後，不要在 `package.json` 修改套件版本，防止在跑 `yarn install` 指令時又噴錯。

### Electron

關於 Electron 是啥便不再贅述。~~要知道的是只有在 Electron 的 Main Process 裡才可以呼叫 node 的 api~~（錯了），所以把 node-oauth 套件放在這跑是沒問題的，但 Renderer Process 才是主要觸發 API 的地方（換頁、捲動、按鈕等）。Electron 提供了 IPC 的 API 界面實作，可以這樣寫：

> commit [df4c8a2](https://github.com/puraku/client/pull/5/commits/df4c8a225a36485747f7022b1391b50ee9e9f19c)

```javascript
// Renderer Process
import { ipcRenderer } from 'electron';

export function request(method, endpoint, params=null) {
  return ipcRenderer.sendSync('puraku:api', {method, endpoint, params});
}

// Main Process
ipcMain.on('puraku:api', (event, args) => {
  const { method, endpoint, params } = args;
  myApiClient.request(method, endpoint, params).then(({data}) => {
    event.returnValue = JSON.parse(data);
  }).catch(error => {
    event.returnValue = { error };
  });
});
```

Renderer Process 送出 API 請求到 Main Process，已經正在監聽的 Main Process 在呼叫完 API 請求(`myAPIClient.request`)之後，返還資料給 Renderer Process。在這裡透過 ipcMain 建立叫做 `puraku:api` 的事件監聽，由 `ipcRenderer` 送出事件請求。`event.returnValue` 是 ipc 的同步寫法。一旦使用同步，整個 Renderer Process 在送出事件請求(`ipcRenderer.sendSync`)之後會被 Block，所以我改用非同步的 IPC 寫法，再用 Promise 封裝。

### Asyncronous IPC

> commit [3f2fee](https://github.com/puraku/client/pull/5/commits/3f2fee64664c4082520a3a8b9ffe6d90cb6cfdbd)

```javascript
// Renderer Process
import { ipcRenderer } from 'electron';

export function request(method, endpoint, params=null) {
  return new Promise((resolve, reject) => {
    const timestamp = Date.now();
    const result = ipcRenderer.send('puraku:api', {method, endpoint, params, timestamp});

    ipcRenderer.once(`puraku:api:${endpoint}:${timestamp}`, (event, result) => {
      if (result.hasOwnProperty('error')) {
        reject(result);
      } else {
        resolve(result);
      }
    });
  });
}

// Main Process
ipcMain.on('puraku:api', (event, args) => {
  const { method, endpoint, params, timestamp } = args;
  myApiClient.request(method, endpoint, params).then(({data}) => {
    event.sender.send(`puraku:api:${endpoint}:${timestamp}`, JSON.parse(data));
  }).catch(error => {
    event.sender.send(`puraku:api:${endpoint}:${timestamp}`, {error});
  });
});
```

這一版跟上面的差別在於，Renderer Process 送請求給 Main Process 之後，馬上建立另一個 IPC 的 Listener 等待 Main Process 回調，而 Main Process 在處裡完 API 請求之後(`myAPIClient.request`) 再用 IPC 非同步寫法回傳資料(`event.sender.send`)。在這個版本 IPC 關係變得比較複雜，簡單畫了一下：

```txt
Promise start  +-----+
                     |           Renerer Process                 Main Process
                     |
                     +-----> +--------------------+
                             |                    |
                             |  ipcRenderer.send  |
                             |                    |         +---------------------+
                             +------------------------------+                     |
                             |                    |         |  API request        |
                             |  ipcRenderer.once  |         |                     |
                             |                    |         |  event.sender.send  |
                             |  create listener   |         |                     |
                             |                    |         |                     |
                             +---------+----------+         +-----------+---------+
                                       |                                |
                                       |                                |
                                       |                                |
                             +---------+----------+                     |
                             |                    <---------------------+
                             |   Event received   |
                             |                    |
                     +-----+ +---------+----------+
                     |                 |
                     |                 |
 Promise end   <-----+                 |
                                       |
                                       |
                                       |
                                       |
                                       |
                                       |
                                       |
                                       |
                                       v
```

### IPC 事件一對一對應

可以發現在這一版的 Renderer Process 我加了一個 `timestamp` 來簡單的區分不同的 API request，因為如果光用 API 的 Endpoint 當做事件的鍵值，戳相同 API 兩次時就會衝到。

```javascript
const timestamp = Date.now();
const eventKey = `puraku:api:${endpoint}:${timestamp}`;
```

不過卻發現每次取的 timestamp 還是有機會一樣，經過 Google 之後我把 `timestamp` 亂數的產生方法改成 `performance.now()`：

```javascript
const randomSeed = performance.now();
const eventKey = `puraku:api:${endpoint}:${randomSeed}`;
```

到這裡，我們就可以用熟悉的 Promise 介面，在 Renderer Process 輕鬆地串接 Main Process 的 API 啦！以下是目前的實作：

```javascript
// Renderer Process
import { ipcRenderer } from 'electron';

export function request(method, endpoint, params = null) {
  return new Promise((resolve, reject) => {
    const randomSeed = performance.now();
    ipcRenderer.send('puraku:api', { method, endpoint, params, randomSeed });

    ipcRenderer.once(`puraku:api:${endpoint}:${randomSeed}`, (event, result) => {
      if (result.hasOwnProperty('error')) {
        reject(result);
      } else {
        resolve(result);
      }
    });
  });
}

// Main Process
const { ipcMain } = require('electron');

ipcMain.on('puraku:api', (event, args) => {
  const { method, endpoint, params, randomSeed } = args;
  myApiClient.request(method, endpoint, params).then(({data}) => {
    event.sender.send(`puraku:api:${endpoint}:${randomSeed}`, JSON.parse(data));
  }).catch(error => {
    event.sender.send(`puraku:api:${endpoint}:${randomSeed}`, {error});
  });
});
```

在 Renderer Process 裡（在我的例子裡是 Vue 前端 App）就可以用簡單的介面來呼叫 API 啦！

## 其它

聽說用 message queue 來實作比較好，不過 It works for now，就暫時沒有更新實作的動力（懶）

可以在 [puraku/client PR#5](https://github.com/puraku/client/pull/5) 閱讀實作的過程。自從對 Redmine 上癮之後，連 GitHub Flow 也一併愛上了，在本 Repo 一個功能就開 Branch 做成 Pull Request，就算一個人的 GitHub Flow 也能玩的愉悅 XD


[plurk-api]: https://www.plurk.com/API
[puraku]: https://github.com/puraku/client
[purakujs]: https://github.com/puraku/purakujs
[plurkjs]: https://github.com/clsung/plurkjs
[node-oauth]: https://github.com/ciaranj/node-oauth
