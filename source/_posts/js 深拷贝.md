title: js  深拷贝问题
date: 2018/10/08
categories:

- 前端
- js
tags:
- 深拷贝
comments: true
---

#### js 深拷贝
js 里面很多东西都是对象，很容易出现引用问题。引用相同的对象多次，且修改时[特别注意js 的深拷贝问题](http://www.cnblogs.com/penghuwan/p/7359026.html)，
1. 下面是我写的错误代码一部分，
```js
pId = {
    12: [{xx:'7', ...}],
    21: [{xx:'6', ...}],
    31: [{xx:'7', ...}]
    }
    starInfos = {
    6: [],
    7: []
    }

    let Activities = []
    for (let pId in posts) {
    let Activity = starInfos[posts[pId][0].xx]));

    Activity.like = posts[pId][0].like;

    Activities.push(Activity);
};
```
2. Activity 实际上是多次引用了starInfos.7，并且对他进行修改，导致Activities 第1/3 元素相同，修改如下：
```js
let Activity = JSON.parse(JSON.stringify(starInfos[posts[pId][0].xx]))));
```
#### js 修改对象
下面是我遇到修改对象的情况， [修改js 对象，合并对象](http://www.cnblogs.com/penghuwan/p/7359026.html)
1. 修改对象
```js
let a = 'gender';
let b = {
    name: 'lcs'
}

b[a] = 'boy'
```
2. 利用 Object.assign(target, source1, source2)
```js
aa = {
    name: 'lcs'
}

bb = {
    gender: 'boy'
}
Object.assign(aa, bb);

aa = {
    name: 'lcs',
    gender: 'boy'
}
```

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！