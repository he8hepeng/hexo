---
title: electron开机自启动
date: 2019-6-23 11:23:31
tags:
  - Electron
---

{% blockquote %}
今天，一大早需求就气势汹汹的来到我身边。我的第一反应。。。这是要谋害朕啊！
果然，客户临时加需求。开机自启动，首次安装增加软件介绍视频以及设置页前置，好嘞 两天后就要~ 走你！
{% endblockquote %}

### 首先 拆解需求
{% blockquote %}
需求是建立在，用户第一次安装。而第一次安装可以通过Electron或者Node去获取
因为我们项目做的是离线版和在线版。所以会在C盘生成.xyzs的文件夹 用来存储json格式的文件数据
也就是说,项目启动,通过node获取用户目录下的.xyzs 如果存在就是安装过 没有就是第一次安装咯 完美解决~（手动删除给我滚）
{% endblockquote %}

```
let fs = require("fs")
let os = require("os")
let path = require("path")
const dirName = os.homedir()
const dirPath = path.join(dirName, '.xyzs')

fs.exists( dirPath, (exists) => {
  console.log(exists ? "存在" : "不存在");
})

```

{% blockquote %}
现在已经可以获取 是否是第一次安装 然后咧
app.vue中，在router-view进行子路由 增加video.vue页面 来播放动画
每次进入页面是 调用electron主线程发送信号，由主线程向Node发送请求 探查是否是第一次安装
再由主线程 event.returnValue 返回结果，来判断是否播放动画及 动画播放完毕执行setting页面的显示
{% endblockquote %}

### 自启动 那就简单了 百度
---
站在巨人的肩膀上成长 才能使我成长的更快
当然 不纯是复制粘贴 读一个博客大概20分钟 理解大概一两个小时 熟练使用需要项目的磨合
我会越来越强的！
---
```
const {Registry} = require('rage-edit')
const path = require('path')
const {app} = require('electron)

module.exports = {
	 
	 url:'HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Run',
	 
	 name:'my_application',
	 
	 //设置启动
	 setAppStart(cbSus,cbErr){
		Registry.set(
			this.url,   //注册表开机启动应用路径
			this.name, //随意写
			app.getPath('exe'), //当前应用路径，也是自动启动的应用路径
			'REG_SZ', // 固定的 
		)
		.then(()=>{
			cbSus && cbSus()
		})
		.catch(()=>{
			cbErr && cbErr()
		})
	},
	
	// 取消启动
	cancelAppStart(cbSus,cbErr){
	  Registry.has(this.url,this.name)
	  	.then((res)=>{
			if(res){
				Registry.delete(this.url,this.name)
					.then(()=>{
						cbSus && cbSus()	
					})
					.catch(()=>{
						cbErr && cbErr()
					})
			}
		})
		.catch(()=>{
			cbErr && cbErr()
		})
	}
}
```