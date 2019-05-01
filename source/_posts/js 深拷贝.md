title: js  promise
date: 2018/9/26
categories:
- 前端
- js
tags:
- js
comments: true
---

在学习

7. [特别注意：js 的深拷贝问题](http://www.cnblogs.com/penghuwan/p/7359026.html)，引用相同的对象多次，且修改时，特别注意
   - 下面是我写的错误代码一部分，
   ```
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
   - Activity 实际上是多次引用了starInfos.7，并且对他进行修改，导致Activities 第1/3 元素相同，修改如下：
   ```
   
    let Activity = JSON.parse(JSON.stringify(starInfos[posts[pId][0].xx]))));
   ```
   
6. [修改js 对象，合并对象](http://www.cnblogs.com/penghuwan/p/7359026.html)
    - 修改对象
    ```
    let a = 'gender';
    let b = {
        name: 'lcs'
    }
    
    b[a] = 'boy'
    
    ```
    - Object.assign(target, source1, source2)
    ```
    aa = {
        name: 'lcs'
    }
    bb = {
        gender: 'boy'
    }
    Object.assign(aa, aa);
    
    aa = {
        name: 'lcs',
        gender: 'boy'
    }
    ```

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！