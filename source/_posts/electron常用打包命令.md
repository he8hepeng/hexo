---
title: electron常用打包命令
date: 2019-5-27 21:21:57
tags:
    - Electron
---

### Electron 常用的打包命令
```
"build": "node .electron-vue/build.js && electron-builder",
"win32": "node .electron-vue/build.js && electron-builder --platform=win32  --arch=ia32",
"mac": "node .electron-vue/build.js && electron-builder --platform=mac ",
"build:dir": "node .electron-vue/build.js && electron-builder --dir",
"build:clean": "cross-env BUILD_TARGET=clean node .electron-vue/build.js",
"build:web": "cross-env BUILD_TARGET=web node .electron-vue/build.js",
"dev": "node .electron-vue/dev-runner.js",
"pack": "npm run pack:main && npm run pack:renderer",
"pack:main": "cross-env NODE_ENV=production webpack --progress --colors --config .electron-vue/webpack.main.config.js",
"pack:renderer": "cross-env NODE_ENV=production webpack --progress --colors --config .electron-vue/webpack.renderer.config.js",
"postinstall": ""
```