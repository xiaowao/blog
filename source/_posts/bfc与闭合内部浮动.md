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
  <font face="微软雅黑" size="2" color="red">注：</font><font face="微软雅黑" size="2">IE6不支持; 一旦子元素的大小超过父容器的大小，就会出显示问题</font>
3.父元素css样式设置 `display: inline-block;`
  <font face="微软雅黑" size="2" color="red">注：</font><font face="微软雅黑" size="2">若父元素使用margin: 0 auto实现居中时，display: inline-block使其转为行内元素，导致居中效果无效</font>
4.父元素css样式设置 `position: absolute` or `position: fixed`
  <font face="微软雅黑" size="2" color="red">注：</font><font face="微软雅黑" size="2">IE6不兼容; 使父元素脱离文档流，清除父元素的居中效果，且对后面的div等有类似于浮动的影响。好似关了一扇窗，却开了一道门，还是尽量不用的好</font>
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
其实，上述方法主要采用了以下两种原理：
- 方法1 - clear
- 方法2-6 - IE专有的hasLayout和W3C标准下的BFC 

这里主要对hasLayout和BFC（毕竟也是大咖呀）展开学习。
#### haslayout
> <font face="微软雅黑" size="2">在IE中，使用layout（布局）概念控制元素的尺寸和定位。当一个元素的 hasLayout属性值为true时，我们说这个元素有一个布局（layout），当一个元素有一个布局时，它负责对自己和可能的子孙元素进行尺寸计算和定位</font>

##### 闭合内部浮动的原理
  通过上述背景我们了解到，当对无layout的元素触发了hasLayout，会使其对自己和子孙元素进行计算，不管子孙元素是否存在浮动。

##### 触发hasLayout的方法
  ```bash
  position: absolute
  float: left|right
  display: inline-block
  width: any value other than 'auto'
  height: any value other than 'auto'
  zoom: any value other than 'normal' （IE专用属性）
  writing-mode: tb-rl（IE专用属性）
  overflow: hidden|scroll|auto（只对IE 7及以上版本有效）
  overflow-x|-y: hidden|scroll|auto（只对IE 7及以上版本有效）
  ```
#### BFC（Block Formatting Context，块级格式化上下文）
> <font face="微软雅黑" size="2"> <font color="#f5871f">Block</font>：box(css的基本布局单位)的一种类型，由display: block | table | list-item控制
> <font color="#f5871f">Formatting context</font>：W3C CSS2.1 规范中的一个概念,表示页面的一个渲染区域，包含一系列渲染规则，用来控制元素及其子元素如何定位，以及与其他元素的作用关系
> <font color="#f5871f">BFC</font>：一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干</font>

##### 渲染规则
- 内部的Box会在垂直方向，一个接一个地放置
- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
- 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)，即使存在浮动也是如此
- BFC的高度时，浮动元素也参与计算
- BFC的区域不会与float box重叠
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之也如此

##### 触发BFC的方法（与触发hasLayout的部分差别来源于浏览器版本问题）
```bash
position: absolute | fixed
float: left|right
display: inline-block | table-cell | table-caption | flex | inline-flex
overflow: hidden|scroll|auto
overflow-x|-y: hidden|scroll|auto
```
<font face="微软雅黑" size="2" color="red">注：</font>
<font face="微软雅黑" size="2">通过解读上述规则，可以发现，BFC除了可以解决闭合浮动的问题外，还可以解决以下问题：
- 闭合浮动
- 同一BFC下，margin重叠问题
- 两栏自适应（左定宽，右自适应）</font>