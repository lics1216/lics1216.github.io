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


#### promise
1. 只关心自身逻辑，不关心结果处理。没有promise 你必须把网络请求success/fail 处理函数写在请求里面，反之，你可以把处理程序写在.then/.catch里面
2. 返回带有 网络请求结果的promise 对象
3. new promise().then() 会返回一个promise 对象，return 和 resolve 差不多一样效果，resovle 是执行成功的返回结果。
```

let a = return http.post(`/api/refresh.php`, data, true).then(res => {
        console.log("reflesh")
        console.log(res.data)
        console.log("reflesh")
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
    
a.then(res)
    
```


欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！