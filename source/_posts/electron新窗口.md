---
title: electron新窗口
date: 2019-6-30 12:20:31
tags:
    - Electron
---

### 新建窗口
---
我们项目 大概有四五个新窗口 太难了 贴段代码散散心，以后就直接复制粘贴就好
---
```
// 引入创建窗口依赖
const {
  BrowserWindow,app
} = require('electron')
const winURL = process.env.NODE_ENV === 'development' ? `http://localhost:9080/#/service` : `file://${__dirname}/index.html#/service`
// 同步其他窗口的创建
const {getPush} = require('../suspensionWindow');
// 生命窗口对象
let PushServiceWindow
const createPushService = function() {
  // 创建窗口
  PushServiceWindow = new BrowserWindow({
    height: 600,
    useContentSize: true,
    frame: false, // 要创建无边框窗口
    width: 360,
    transparent: true,
    resizable: false,
    alwaysOnTop: false,
    type: 'toolbar'
  })
  // 获取窗口信息
  const screen = require('electron').screen;
  const size = screen.getPrimaryDisplay().workAreaSize;
  // 定位窗口位置
  PushServiceWindow.setPosition(size.width - 370, 2000);
  PushServiceWindow.loadURL(winURL)
  // PushServiceWindow.webContents.openDevTools();
  // PushServiceWindow.webContents.openDevTools()
  // 同步其他窗口的创建
  PushServiceWindow.on('ready-to-show', () => {
    getPush.getMesuspensionWindow()
  })
  // 关闭操作 将对象销毁
  PushServiceWindow.on('closed', () => {
    PushServiceWindow = null
  })

  // PushServiceWindow.on('blur', () => {
  //   PushServiceWindow.webContents.send('appblur')
  // })

  // PushServiceWindow.on('focus', () => {
  //   PushServiceWindow.webContents.send('appfocus')
  // })
}

// 当其他窗口需要获取本窗口对象时 将对象动态传回去
const PushServiceWindowObj = function () {
  return PushServiceWindow;
}

exports.PushServiceWindowObj = PushServiceWindowObj;
exports.createPushService = createPushService;

```
