---
title: echarts导出图片
date: 2019-12-29 12:40:45
tags:
   - Echarts
---

### Echarts 导出图片
```
  downCharts () {
    let baseURL
    if (this.btShow) {
      baseURL = this.$refs.pie.getImageBase()
    } else {
      baseURL = this.$refs.bar.getImageBase()
    }
    const elink = document.createElement('a');
    elink.download = '统计图';
    elink.style.display = 'none';
    elink.href = baseURL;
    document.body.appendChild(elink);
    elink.click();
    URL.revokeObjectURL(elink.href); // 释放URL 对象
    document.body.removeChild(elink)
  }



  getImageBase () {
    return this.myChart.getDataURL({
        type: 'png',
        pixelRatio: 1.5,  //放大两倍下载，之后压缩到同等大小展示。解决生成图片在移动端模糊问题
        backgroundColor: '#fff'
    })
  }
```