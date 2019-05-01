title: css 给元素加边框和蒙版
date: 2018/10/01
categories:

- 前端
- css
tags:
- 边框
comments: true
---

#### css 加边框
利用css 给元素加上边框，可以利用下面两种方式实现
1. border 属性，设定边框宽度，假设要实现上下边框
```css
border: 1rpx solid rgba(0, 0, 0, 0.15);
border-width: 0rpx 0rpx 1rpx 0rpx;
```
2. [设置 box-shadow ](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)， 阴影矩形会往上下移动 1rpx
```css
/* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
<!-- 这个元素阴影盒子的偏移，正负含义类似 定位的left/top-->
box-shadow: 0rpx -1rpx 0rpx 0rpx rgba(0, 0, 0, 0.15);

<!-- 把阴影延y 轴下移1rpx  -->
box-shadow: 0px 1px 0px 0px rgba(0, 0, 0, 0.15)
```

#### css 加蒙版
1. 滤镜、透明度实现给元素加一层 蒙版
```css
<!-- 透明度 -->
opacity: 0.7

<!-- 滤镜-->
filter: grayscale(50%)

```

这就是一些关于学习css 的笔记，欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！