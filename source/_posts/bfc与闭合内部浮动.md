---
title: haslayout, bfc 闭合内部浮动
date: 2019-02-25 15:21:12
tags: css 布局
---

### 闭合内部浮动的方法

作为前端的好孩子，总会遇到一个经典不朽的问题：有几种闭合内部浮动的方法？

1.在最后一个浮动元素的下面添加空白标签
  ```html
  <!-- 方法1 -->
  <div style="clean:both;"></div>
  <!-- 方法2 -->
  <br clear="all">
  ```
2.父元素css样式设置 `overflow: hidden;`
  - IE 6不支持
  - 一旦子元素的大小超过父容器的大小，就会出显示问题
3.父元素css样式设置 `display: inline-block;`
  - 若父元素使用margin: 0 auto实现居中时，display: inline-block使其转为行内元素，导致居中效果无效
4.父元素css样式设置 `position: absolute` or `position: fixed`
  - IE6不兼容
  - 使父元素脱离文档流，清除父元素的居中效果，且对后面的div等有类似于浮动的影响。好似关了一扇窗，却开了一道门，还是尽量不用的好
5.父元素css样式设置 `float：right` or `float: left`
6.为父元素添加after伪类
  ``` css
  // IE8+或其他-方法1
  .clearfix:after {
      display: block;
      content: " ";
      height: 0;
      overflow: hidden;
      clear: both;
  }
  // IE8+或其他-方法2
  .clearfix:after {
      display: table;
      content: " ";
      clear: both;
  }
  //IE6、IE7
  .clearfix {
    *zoom: 1;
  }
  ```

### 闭合浮动的原理

在在整理的过程中，我们应该思考一下这背后的运行原理，否则的话，这些零碎的方法依靠硬记简直是伤害脑细胞和消磨青春的利器啊~~~

其实，这里主要应用了以下两种方法：

1.clear(方法1)
2.IE专有的hasLayout和W3C标准下的BFC （方法2-6）

这里主要对hasLayout和BFC（毕竟也是大咖呀）展开学习。
1 背景
  在IE中，使用layout（布局）概念控制元素的尺寸和定位。当一个元素的 hasLayout属性值为true时，我们说这个元素有一个布局（layout），当一个元素有一个布局时，它负责对自己和可能的子孙元素进行尺寸计算和定位

2 闭合内部浮动的原理
  通过上述背景我们了解到，当对无layout的元素触发了hasLayout，会使其对自己和子孙元素进行计算，不管子孙元素是否存在浮动。

3 触发hasLayout的方法
  