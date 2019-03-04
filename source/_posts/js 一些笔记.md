title: js 一些笔记（一）
date: 2018/9/26
categories:
- 前端
- js
tags:
- js
comments: true
---

在学习js 开发前后端的过程中，感觉刷新了自己的认知。很多以为只有其他语言才能完成的功能，js 却也实现了，虽然封装的过程不一样。真的慢慢意识到语言是工具。

##### js 一些笔记
学习js 语法，和其他接触过的语言对比一下
1. js es6增加了map/set 数据结构，遍历对象有for--of 和 forEach方法
2. js和java 不同，可以解构传参
3. c++/java 里面的对象，一定得先定义class，并且class里面要有属性，方法等。但是js定义对象，类似json
    - vue 页面的Vue对象就种写法
    - 微信小程序的page 对象也是这种写法
4. java是静态的，js是动态的。可以动态给对象添加属性和函数，甚至给内置的函数重新指向新函数。
    - 动态指定属性, 假如你想实现一个简单的对象双向指向
    ```
     var a = ['a', 'b', 'c'];
     a.name = 'lcs';
     
     var b = ['d', 'e']
     
     a.objectB = b;
     b.objectA = a;
    ```
    - 你想实现函数的重载，**用于调试**或者比如，简单统计一下该函数调用次数, parseInt 是window对象的方法
    ```
    var count = 0;
    var oldParseInt = parseInt;
    
    window.parseInt = function(){
        count += 1;
        return oldParseInt.apply(null, arguments);
    };
    
    //test
    parseInt('10');
    parseInt('10');
    
    console.log('xx', count);  //2
    ```
5. js的函数式编程(return 函数或者函数为参数)，使得对象里面的this指向变得复杂起来；java没有函数式编程，没有this 的指向问题
    - 返回函数 或者函数内再定义函数，这两种情况都很容易引发this坑
    - var that = this/apply
    - 箭头函数(常用)，this和词法作用域绑定，即外层调用者
    ```
    var obj = {
        birth: 1990,
        getAge: function () {
            var b = this.birth; // 1990
            var fn = () => {
                new Date().getFullYear() - this.birth;
                // this指向obj对象
            }
            return fn();
        }
    };
    obj.getAge(); // 25
    ```
6. js和python 一样有高阶函数，对数据处理起来更方便
    - map在Array 里面，对Array每一个元素做同样处理，但一般需求是不同处理
    - reduce，感觉不常用，可用来求和
    - filter, 过滤Array 元素, return true 保留，否则删除
    ```
    var a = ['a', 'b', 'c'];
    a.filter(function(element, index, self){
        // element  a b c
        // index 0 1 2
        // slef Array本身 a
        return true // 保留，false删除
    })
    ```
    - sort 默认把Aarry 元素转换为string 再比较大小
        - 高阶函数，函数传入函数
        ```
        var a = [{
                  age: 12,
                  ts:'12121232'
                 },{
                  age: 132, 
                  ts:'32121232'
                 },{
                  age: 132, 
                  ts:'121231232'
                 }]
        a.sort(function(a, b){
             return  1  // b a 返回正值，不管你搞什么，我只认正负
             return  0  // b=a 
             return -1  // a b 返回负值
        })
        ```
7. js 闭包功能
    - 函数fun1里面可以再定义一个函数fun2，若fun2使用了fun1的变量，通过调用fun1返回fun2，以后延迟再fun2()，变量没有出问题了。这就是闭包
    - 闭包的功能，返回一个函数然后延迟执行
8. js/python 的生成器功能类似
9. js/python 装饰器
10. 面向对象编程，js 除了用{ }来定义对象外，也可以用new 关键字来实现。但是在es6之前不像c++/java 语言定义对象有class 关键字，定义对象和对象的继承都必须要求正确实现原型链，继承是原型链的继承。
    - 创建对象
    - 为了实现创建多个对象，共用一个hello函数，在原型链上做调整
    ```
    function Student(name) {
        this.name = name;
        this.hello = function () {
            alert('Hello, ' + this.name + '!');
        }
    }

    var xiaoming = new Student('小明');
    var xiaohong = new Student('小红');
    xiaoming.name; // '小明'
    xiaohong.name; // '小红'
    xiaoming.hello; // function: Student.hello()
    xiaohong.hello; // function: Student.hello()
    xiaoming.hello === xiaohong.hello; // false
    
    //调整原型链
    function Student(name) {
        this.name = name;
    }
    
    Student.prototype.hello = function () {
        alert('Hello, ' + this.name + '!');
    };
    ```
    - 继承，没有class 关键字，思路是在原型上实现继承，代码量大
    - es6 Class 继承，对原型继承实现了封装，es6封装了原型
    ```
    class Student {
        constructor(name) {
            this.name = name;
        }
    
        hello() {
            alert('Hello, ' + this.name + '!');
        }
    }
    
    var xiaoming = new Student('小明');
    xiaoming.hello();
    
    class PrimaryStudent extends Student {
        constructor(name, grade) {
            super(name); // 记得用super调用父类的构造方法!
            this.grade = grade;
        }
    
        myGrade() {
            alert('I am at grade ' + this.grade);
        }
    }
    ```
    - js 的面向对象编程应用还有待体会
11. js 对不存在的属性返回 undefined
    - 利用||赋值是常用方法
    ```
    let lcs = 1 || 2 ;          // lcs = 1;
    let lcs = undefined || 2;   // lcs = 2
    ```
    - null/undefined if判断没有问题
    ```
    if(undefined){
      console.log('undefined if')
    }else{
      console.log('undefined else')
    }
    
    if(null){
      console.log('null if')
    }else{
      console.log('null else')
    }
    ```
12. js 可以获取浏览器提供的很多对象，获得浏览器的属性
13. html 文档被浏览器解析后是dom 结构，js操作dom，改变页面结构。Ajax/JQuery 都有了解，
14. Promise, 使得发送请求的逻辑和处理返回的结果分开为异步。因为一般来说js 代码是单线程的，必须等待请求返回结果处理好再执行 todo2 
    - success和fail 会在 todo1和todo2 结束再执行
    ```
    xxxxx todo1
    
    function put(url, data) {
        return new Promise(function(resolve, reject) {
            request({
                url: url,
                method: 'PUT',
                data: data,
                success: function(res) {
                    resolve(res.data)
                },
                fail: function(err) {
                    reject(err)
                }
            })
        })
    }
    
    xx todo2
    
    ```


参考文章
>1. [js教程--by 廖雪峰](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)
>2. [es6教程--by 阮一峰](http://es6.ruanyifeng.com/)


欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！