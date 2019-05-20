---
title: Dom增删改查隐藏技能
date: 2019-02-25 15:21:12
tags: Dom js
---

### 查询元素在视口中得绝对位置

``` js
function getAbsolutePos(dom) {
  var curLeft = dom.offsetLeft || 0
  var curTop = dom.offsetTop || 0
  while (dom.offsetParent) {
    dom = dom.offsetParent
    curLeft += dom.offsetLeft
    curTop += dom.offsetTop
  }
  return [curLeft, curTop]
}
```