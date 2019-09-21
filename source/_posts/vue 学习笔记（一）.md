title: vue 学习笔记（一）
date: 2018/9/27
categories:
- web
- vue
tags:
- vue
comments: true
---

vue.js 是实现双向绑定mvvm 的框架，用它写项目可以实现前后端分离。起初误以为它是类似easyUI/bootstrap这样的前端UI框架，接触之后发现并不是，它是为了使得页面组件化。

#### Vue 起步
首先[学习Vue 官方文档](https://cn.vuejs.org/v2/guide/index.html)，看到组件基础那边。感受一下它的双向绑定，加上之前接触了es6 就不会觉得vue 语法那么奇怪。vue 一些笔记
1. mvvm，一旦 vue实例里面的 data改变，会重新渲染一遍页面
2. 当需对后端返回数据做处理再show时，利用计算属性、侦听属性来实现
3. 既然能动态绑定数据，那标签的class 自然也可以
4. vue.js 渲染页面时，常见的情况
   * 条件渲染
   * 列表渲染（for 里面有个恶心的key）
5. 事件处理（点击，按键，鼠标），注意事件修饰符，就是事件函数本身有些问题要处理，如事件只能点击一次，事件截流...，但是这个和业务逻辑没有关系，不能把处理程序写在事件函数里面，通过事件修饰符来实现
```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
```
6. 常常需要获取用户操作页面上的很多控件如input/textarea/checkbox/radio/option的结果, 利用vue 的v-model 指令
7. 组件
    - 在组件上的一些自定义事件实现，涉及input 的v-model
    - 组件的slot 插槽语法，微信小程序自定义组件就是利用了slot
    - 动态组件，vue组件实现tab 更换显示不同的组件

#### 写vue+koa的项目
可以写一个简单项目来体会一下vue 的语法，需要涉及下面知识点

`module.exports`，当js 代码越写越多的时候能不能像java 一样利用import 来分模块引入，js 利用函数式编程和闭包特性来实现这一想法。
1. [js 模块实现--by 廖雪峰](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434502419592fd80bbb0613a42118ccab9435af408fd000)

axios，对前端向服务器发起请求的代码进行封装
1. [Promise 理解](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014345008539155e93fc16046d4bb7854943814c4f9dc2000)
2. [axios 使用](https://cn.vuejs.org/v2/cookbook/using-axios-to-consume-apis.html)

npm，这是js 的包管理工具，类似java maven。只要在项目的package.json 文件中配置依赖，输入命令就会自动下载。
1. [npm 入门--菜鸟](http://www.runoob.com/nodejs/nodejs-npm.html)
2. [npm 常用命令](http://www.cnblogs.com/PeunZhang/p/5553574.html#npm-package.json)
3. [npm package.json字段说明](https://github.com/ericdum/mujiang.info/issues/6/) 

koa，是基于node.js 的web框架，对http进行了封装。可以用它来处理前端发来的请求。
1. [koa 入门](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434501579966ab03decb0dd246e1a6799dd653a15e1b000)

#### 遇到的问题
结合以上技术vue+koa，简单的实现了blog 首页。效果如下图，勿喷，[**代码在这里**](https://github.com/lics1216/web_shijian/tree/vue+koa)

![vue+koa](/images/20180927/vue+koa.png)

实现过程中遇到一些问题
1. 命令node app.js 实例化koa对象可以监听端口，为什么跳转不到static/index.html 页面上
    - 加载静态资源，用/index.html 即可访问，不用前缀static
    ```
    const serve = require('koa-static');
    const main = serve(path.join(__dirname, 'static'));
    ```
2. 如何返回json 字符串
    - "mount" 和 mount 是一样的，把js对象转成字符串
    - json 原本就是字符串
    ```
    var content_sort = async (ctx, next) => {
        ctx.response.type = 'json';
        let data = [
            { mount: 3, sort: "日志" },
            { mount: 5, sort: '分类' },
            { mount: 3, sort: '标签' }
        ];
        ctx.response.body = JSON.stringify(data);
    };
    ```
3. 为什么前端用v-for 渲染后，会生成html 标签，但是看不到
    - html 没有<text>标签；微信小程序有，我靠。改为span
    ```
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    
    <div id="app">
        <div v-for="sort of sorts" :key="sort.id">
            <text>{{sort.mount}}</text>
            <text>{{sort.sort}}</text>
        </div>
    </div>
    
    <script>
        new Vue({
            el: '#app',
            data: {
                sorts: [        
                    { mount: 3, sort: "日志" },
                    { mount: 5, sort: '分类' },
                    { mount: 3, sort: '标签' }
                ]
            }
        });
    </script>
    ```

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！

