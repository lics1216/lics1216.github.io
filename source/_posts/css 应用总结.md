title: css 应用总结
date: 2018/9/29
categories:

- web
- css
tags:
- 伪类
- 动画
- 边框
comments: true
---

#### css伪类
下面是自己使用过 伪类的几种场景

##### :last-child
利用 css:last-child，实现循环四个元素，每一个元素都有底边框，但最后一个元素没有
1. 注意 xx:last-child 找到有共同父节点样式为xx 的元素
2. 再选定最后一个元素，该元素必须处在最后一位，且不能有其他不相关元素

比如复杂一点的形式：
```html
<view>
    <view class="xx">
        <text>tt</text>
        <view class='yy'>bb</view> 
    </view>

    <view class="xx">
        <text>tt</text>
        <view class='yy'>bb</view> 
    </view>

    <view class="xx">
        <text>tt</text>
        <view class='yy'>bb</view> 
    </view>
<view>
```
css 代码应该这样写，
```css
.xx .yy {
    border: 1rpx solid #a7a7a7;
    border-width: 0 0 1rpx 0;
}

.xx:last-child .yy {
    border-width: 0;
}
```

#####  :nth-child(3n+2)
利用 :nth-child(3n+2) 实现一个九宫格，用于展示图片。实现效果如下，

![九宫格](/images/20180929/九宫格.png)

页面利用flex 布局，标签代码
```html
<view class='activities'>
    <view wx:for='{{starActivity}}' wx:key='postId' class='activity'>
        <image class='img-activity' src='xx' mode='aspectFill'></image>
        <xx></xx>
        <xx></xx>
    </view>
    
</view>
```
```css
.activities {
    width: 750rpx;
    padding: 30rpx;
    display: flex;
    flex-wrap: wrap;
    box-sizing: border-box;
}

.activity {
    width: 32%;
    height: 226rpx;
    margin-bottom: 15rpx;
    border: 1rpx solid #eee;
    box-sizing: border-box;
}

.activity:nth-child(3n+2){
    margin-left: 15rpx;
    margin-right: 15rpx;
}

.activity .img-activity{
    width: 100%;
    height: 100%;
}
```
注意，img-activity 宽高设置为 100%，是指子元素 内容宽高==父元素 内容宽高，至于内容宽高指什么，是由box-sizing 值确定的（默认值 content-box)。
1. box-sizing: border-box     设置的width/height == 内容宽高 + padding + border 
2. box-sizing: content-box    设置的width/height == 内容宽高

在这里 activity 的内容宽高 = wh - (padding + border)， 所以img wh = 226rpx - 1rpx

#### css 动画
很多时候页面要添加动画，优化用户体验，可以利用css 语法支持实现。[css 动画，不是瞬间变动，而是有个缓动效果](http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html)，实现效果

![动画图](/images/20181018/animation1.gif)

实现效果，需要在点击后给元素添加一个样式修饰，下次点击又重现效果，类似 xx:hover。微信小程序元素（view/button）的 hover-class 刚好支持。实现如下
```css
<view class="action-sort-mount" hover-class='scale'>
    <image class="user-like img-WH" src="{{isLike?'/images/like_on.svg':'/images/like.svg'}}" />
    <text class='text1'>{{xx}}</text>
</view>

.scale {
    animation: 0.3s myScale;
}

@keyframes myScale {
    from {
        transform: scale(0.5, 0.5);
    }

    to {
        transform: scale(1, 1);
    }
}
```

#### transform: translate(x, y)
利用  transform: translate(x, y) 实现一张 图片在底下一张图片（宽高未知）居中的效果，

![动画图](/images/20181018/图片中心居中.png)

步骤如下，
```css
<view class='video-one-poster-icon'>
    <image class='video-one-activity' src='{{xx}}' mode="aspectFill"/>
    <view class='bg'></view>  
    <image class='video-icon' src='/images/video-icon.svg' />
</view>


.bg{
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    background: #000;
    opacity: 0.3;
}

.video-icon{
    width: 100rpx;
    height: 100rpx;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
```

善于利用 transform 改变元素大小，移动位置。

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

这就是一些关于学习css 的体会，欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！