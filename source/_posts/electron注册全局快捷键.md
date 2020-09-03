---
title: electron注册全局快捷键
date: 2019-5-29 29:03:24
tags:
    - Electron
---


### Electron 注册快捷键事件
```
ipcMain.on('changeWebShot', (event, args) => {
  console.log(args)
  globalShortcut.unregister(args[0]); // 注销上一次的热键
  // 注册新的全局热键
  globalShortcut.register(args[1], () => {
    // 截屏
    Promise.race([operation.webShot()]).then(() => {
      event.returnValue = true;
    });
  })
  event.returnValue = true;
})
```