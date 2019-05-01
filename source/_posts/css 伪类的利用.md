title: css 伪类的利用
date: 2018/9/29
categories:

- 前端
- css
tags:
- 伪类
comments: true
---

下面是自己使用过 伪类的几种场景

#### :last-child
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

####  :nth-child(3n+2)
利用 :nth-child(3n+2) 实现一个九宫格，用于展示图片。实现效果如下，

![九宫格](/images/20190929/九宫格.png)

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


这就是一些关于学习css 的体会，欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！