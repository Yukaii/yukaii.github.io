---
layout: post
title: "在 Electron 的 Renderer Process 裡，使用 IPC 呼叫跑在 Main Process 的 API"
---

這次我最近在實作噗浪 electron app - [**Puraku**][puraku] 時使用的抽象化寫法。

先談一下背景。其實在 [Plurk API][plurk-api] 的網頁上就已經有 Plurk 的 [JavaScript API Library][purakujs]，不過它沒有包成 npm 可以直接使用，而且也是相依於 [node-oauth][node-oauth] 套件，看名字就知道和 Node 有關。

這次寫的 [puraku][puraku] 是一個以前端為主的桌面軟體，所以我勢必要對 API Client Library 做些改寫。

## 重新封裝 API Library

請直接參考 [purakujs][purakujs]，也是個可以直接使用的 Plurk API Node library，雖然這年頭也沒多少工程師在串噗浪了 😅 。值得一提的是 yarn 對 `npm link` 的支援不太好，在設定本機開發環境跑完 `npm link` 後，不要在 `package.json` 加上套件名稱，防止在跑 `yarn install` 指令時又噴錯。

### Electron

關於 Electron 是啥便不再贅述。首先在 Electron 的 Main Process 可以呼叫 node 的 api，所以把 oauth library 放在這跑是完全沒問題的，但 Renderer Process 才是主要觸發 API 的地方。Electron 提供了 IPC 的 API 界面實作，可以這樣寫：

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

這是 ipc api 的同步寫法。不過一旦使用同步，整個 Renderer Process 會被 Block 掉，所以我們要改用非同步的 message passing，以及相應的 Promise 封裝。

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

### IPC 事件一對一對應

可以發現在這一版的 Renderer Process 我加了一個 `timestamp` 來簡單的區分不同的 API request，因為如果光用 API 的 Endpoint 當做事件的鍵值，戳相同 API 兩次時就會衝到。

```javascript
const timestamp = Date.now();
const eventKey = `puraku:api:${endpoint}:${timestamp}`;
```

不過卻發現每次取的 timestamp 還是有機會一樣，經過 Google 之後我們把 `timestamp` 亂數的產生方法改成 `performance.now()`：

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

## 其它

聽說用 message queue 來實作比較好，不過 It works for now，就暫時沒有更新實作的動力（懶）

可以在 [puraku/client PR#5](https://github.com/puraku/client/pull/5) 閱讀實作的過程。在本專案，我堅持一個功能就開 Branch 做成 Pull Request，可謂單人的 GitHub Flow XD


[plurk-api]: https://www.plurk.com/API
[puraku]: https://github.com/puraku/client
[purakujs]: https://github.com/puraku/purakujs
[plurkjs]: https://github.com/clsung/plurkjs
[node-oauth]: https://github.com/ciaranj/node-oauth
