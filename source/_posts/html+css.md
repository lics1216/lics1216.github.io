title: html/css 写页面
date: 2018/9/25
categories:

- 前端
- html/css
tags:
- css
comments: true
---

用html/css 怎样写出页面呢，看到每个页面都有很多html标签和css 修饰属性，难免会被吓到。其实我们只要从整体到细节，即页面结构到元素来理解页面是如何组合在一起的，就可以入门。

#### 页面是如何组合在一起的
首先页面是由html 标签组成的，每个标签都代表一个盒子（如下图），盒子有外边距、边框、内边距、内容宽高等属性。

![box](/images/20180925/box.png)

1. 标签之间可以嵌套
2. display，决定标签是否独占一行，或者由自身宽高决定所占位置
3. position，标签在页面位置原本是从上到下排列，但可以改变

比如，下面代码
```
<div class='border-show'>
    div 
</div>
<div class='border-show'>
    div 
</div>
<span class='border-show'>span</span>
<span class='border-show'>span</span>

<style>
   .border-show {
       border: 1px solid #000;
       margin: 50px;
       padding: 1px;
   }
</style>
```
效果如下，

![div_span](/images/20180925/div_span.png)

display 属性规定元素应该生成的框的类型，明显div 该属性值为block，span 是inline（这是默认值）。可以通过修改display 来改变div 框的类型，常用属性值如，block/inline-block/flex。常用的块级元素有div p form ul ol li等，行内元素span strong等。

position 属性指定一个元素（静态的，相对的，绝对或固定）的定位方法的类型，常用的有：
1. static，默认值，就是按照从上到下的顺序排列页面元素
2. fixed，指该元素，脱离原本的从上到下的顺序，只以浏览器窗口为参照物，通过 left/top/right/bottom 来控制和浏览器的相对位置。stickly 类似
3. relative，指该元素，相对从上到下的原本位置进行移动，通过left/top/right/bottom 来控制
4. absolute，指该元素，相对于第一个含（position：除static 外）的父元素进行移动

left/top/right/bottom 的正负值含义如下图

![position](/images/20180925/position.png)

#### 用什么布局写页面



#### 整体到细节







这就是一些关于html/css 写页面的体会，欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！