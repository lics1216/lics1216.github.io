title: css 动画
date: 2018/10/18
categories:

- 前端
- css
tags:
- 边框
comments: true
---

很多时候页面要添加动画，优化用户体验，可以利用css 语法支持实现。
#### css 动画
[css 动画，不是瞬间变动，而是有个缓动效果](http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html)，实现效果

![动画图](/images/20181018/animation.gif)

实现效果，需要在点击后给元素添加一个样式修饰，下次点击又重现效果，类似 xx:hover。微信小程序元素（view/button）的 hover-class 刚好支持。实现如下
```html

```

#### transform
利用 transform 实现一张 图片在底下一张图片（宽高未知）居中的效果，

![动画图](/images/20181018/图片中心居中.png)

步骤如下，
```css

```

总结善于利用
1. 比例

这就是一些关于学习css 的笔记，欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受!