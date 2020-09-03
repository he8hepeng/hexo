---
title: axios下载错误提示
date: 2020-01-07 10:33:59
tags:
    - axios
---
### Blob转换为JSON
---
今天，开发小伙伴问我。当导出失败的时候 后台返回的Message 前台没有打印。我是在 response中进行的拦截
发现 responseType: 'blob'的返回值 是要给blob的类 并没有message啊。
从期望来看。是要将blob对象转换成 JSON 就可以了
---
#### 分析
---
但是 我所封装的axios组件中 正常的get post请求和 blob请求等 是公用一个拦截器的。
为了不影响其他功能。就需要做一次判断。
1.首先判断是否是blob请求
2.其次判断 type类型是否是 'application/json'
都满足是 即可转换打印
---

```
if (
  error.config.responseType === 'blob' && 
  error.response.data.type === 'application.json'
  ) {
  let reader = new FileReader()
  reader.readAsText(error.response.data, 'utf-8')
  reader.onload = e => {
    Message.error(JSON.parse(reader.result))
  }
}
```


