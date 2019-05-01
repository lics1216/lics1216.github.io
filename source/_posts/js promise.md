title: js  promise 理解
date: 2018/9/26
categories:
- 前端
- js
tags:
- promise
comments: true
---

#### promise
promise 只关心自身逻辑，不关心结果处理。没有promise 你必须把网络请求success/fail 处理函数写在请求里面，反之，你可以把处理程序写在.then/.catch里面。这样把网络请求和处理请求结果的逻辑分开，使代码结构更清晰。比如，
1. 模拟请求代码，下面是一个向服务端发起请求的函数
```js
function test(resolve, reject) {
    var timeOut = Math.random() * 2;
    log('set timeout to: ' + timeOut + ' seconds.');
    setTimeout(function () {
        if (timeOut < 1) {
            log('请求成功...');
            resolve('200 OK');
        }
        else {
            log('请求失败...');
            reject('timeout in ' + timeOut + ' seconds.');
        }
    }, timeOut * 1000);
}
```
2. 处理请求结果的逻辑，可以和请求函数分开
```js
var p1 = new Promise(test);
var p2 = p1.then(function (result) {
    console.log('成功：' + result);
});
var p3 = p2.catch(function (reason) {
    console.log('失败：' + reason);
});
```
注意的是，
1. 下面代码返回的是，带有网络请求结果的promise 对象
```js
return new Promise(function(resolve, reject){
    xxx
});
```
2. new promise().then() 会返回一个promise 对象，return 和 resolve 差不多一样效果，resovle 是执行成功的返回结果。
```js
let a = return http.post(`/api/refresh.php`, data, true).then(res => {
        return res.data;
    })

a.then(res =>{
   // res 就是上面的res.data
})

<!--若改为-->

let a = return http.post(`/api/refresh.php`, data, true).then(res => {
        return new Promise(function(resolve, reject){
            resolve('lcs')
        });
    })
    
a.then(res => {
    // res 还是一个Promise 对象
})
```

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！