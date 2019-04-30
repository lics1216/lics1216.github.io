title: css 动画
date: 2018/10/01
categories:

- 前端
- css
tags:
- css
comments: true
---

下面是自己使用过 伪类的几种场景

19. [css 动画，不是瞬间变动，而是有个缓动效果](http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html)
    - 点击后，怎样很好的控制css，类似 xx:hover
    - 微信小程序 就用 hover-class ，view/button 支持

#### css
学习到的一些css 属性
1. [box-shadow](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)

    - 阴影矩形会往上移动 1rpx
    ```
    /* x偏移量 | y偏移量 | 阴影模糊半径 | 阴影扩散半径 | 阴影颜色 */
    box-shadow: 2px 2px 2px 1px rgba(0, 0, 0, 0.2);
    
    <!-- 这个元素阴影盒子的偏移，正负含义类似 定位的left/top-->
    box-shadow: 0rpx -1rpx 0rpx 0rpx rgba(0, 0, 0, 0.15);
    ```
css 
1. 加底边框
```
<!-- 边框 -->
border: 1rpx solid rgba(0, 0, 0, 0.15);
border-width: 0rpx 0rpx 1rpx 0rpx;

<!-- 把阴影延y 轴下移1rpx  -->
box-shadow: 0px 1px 0px 0px rgba(0, 0, 0, 0.15)

```

2. 滤镜、透明度实现给元素加一层 蒙版
```
<!-- 透明度 -->
opacity:0.5

<!-- 滤镜-->
filter: 

```

这就是一些关于学习css 的体会，欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！