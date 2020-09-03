---
title: axios全局遮罩
date: 2020-01-07 11:11:06
tags:
    - axios
---
### 全局遮罩
---
因为全局的axios组件 所以很容易的从前置和后置拦截器中
通过headers向后台传输 token或者 user-info
并使用elementUi的 Loading.service 进行了请求的遮罩
本来是很舒服的一件事
但突然发现 因为业务需要 使用Promise.all 的遮罩缺出现了问题
---

#### 分析原因
---
因为大家都知道 .all 是当所有请求都success时 按照书写顺序同时返回。
但拦截器并不知道，当第一个接口开启拦截器后，后续接口同步开启。
但第一个结束的接口关闭了遮罩。其他接口却还没有完成。
---

#### 对策
---
增加计数器 每次开启遮罩进行计数
关闭之后 进行-- 当等于0时即可默认所有接口完成 关闭遮罩
---

```
请求拦截器
// 为了解决 promise.all的 多个参数 无法计算遮罩 增加遮罩计数器 by--刘春阳 徐速成
if (modelIndex === 0) {
  loadingInstance = Loading.service({
    lock: true,
    text: '努力拉取中 ~>_<~',
    background: 'rgba(0, 0, 0, 0.7)'
  })
}
modelIndex++
```
```
响应拦截器
modelIndex--
if (modelIndex === 0) {
  if (loadingInstance) {
    loadingInstance.close()
  }
}

```
