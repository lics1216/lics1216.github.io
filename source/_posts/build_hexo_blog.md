title: hexo 搭建自己的blog
date: 2018/8/18
categories:
- 基础
- blog
tags:
- hexo
- git
comments: true
---
不喜欢新浪、csdn、知乎，简书的博客平台，因为页面有广告，就用hexo框架配合github 搭建了一个自己的blog。希望将自己学习的技术经历总结下来，沉淀自己，也借此和大家一起分享、探讨。

#### hexo 搭建博客技能需要
1. 了解 git 常用命令和概念
2. 了解 linux 常用命令

#### 安装准备
博客页面是先用 md 语法编辑成为文，再由node.js 生成静态html而来，最后部署到github供大家访问。所以需在本地[安装](https://hexo.io/zh-cn/docs/)
1. Node.js
2. Git

#### 发布文章
[参考hexo的官方中文文档](https://hexo.io/zh-cn/docs/)，相信你一定能够顺利完成部署之前的任务，本地博文的编辑。一般我的做法是，
1. 利用md 编辑器把博文编辑好
    - 参考官方文档md 文件的书写格式
    - 在博文开头设置 title/data/categories/tags相关属性，如：
    ```
    title: hexo 搭建自己的blog
    date: 2018/8/18
    categories:
    - 基础
    - blog
    tags:
    - hexo
    - git
    comments: true
    ---
    ```
2. 把md 文件copy 至博客根目录的source/_post目录下
3. 在博客根目录打开Git 命令行
    - hexo g
    - hexo server
4. 在浏览器通过 http://localhost:4000/ 访问博客
5. 若你想删除博文
    - hexo clean
    - 进入 source/_post 删除该md 文件

通过浏览器访问本地博文，修改至感觉满意之后，可以部署到github 供其他开发人员访问。一般步骤：
1. 在你的github 建立仓库
    - 仓库名，[github username].github.io
    - 如：lcs1234567.github.io
2. 编辑 本地博客根目录下的_config.yml
    - 设置url: https://[github username].github.io
    - 在文件最后添加 deploy 信息， 注意空格。如：
    ```
    deploy:
      type: git
      repo: git@github.com:lcs1234567/lcs1234567.github.io.git
      branch: master
    ```
3. 由于部署项目到github 用的是ssh协议，非https，需配置私公钥
    - 随便那个目录 打开git 命令行
    - ssh-keygen -t rsa
    - 一路回车键，待私公钥生成，打开存放目录
    - 将公钥复制，添加到你的github，点击头像/setting/SSH and GPG keys 
4. 执行部署命令，本地git命令行
    ```
	hexo d
    ```

此时可以通过 https://[github username].github.io 访问你的博客！
	
#### 设置Next 主题
设置博客的主题，我当前选择next，[参考 next 文档-中文](https://theme-next.iissnan.com/getting-started.html)。这种美化页面的细活，自己耐心做就行了。

#### 实现 多台电脑协作
如果你在家用自己的电脑编辑好博文，并通过`hexo g -d`部署到github；后面你又想在公司写博客、部署，但发现github所存并非源代码，而是生成后的静态文件。

解决这个问题，我一般是在github建立两个分支，打开你的[github username].github.io仓库，当前分支是master，但这是生成后的静态文件。这是因为你在_config.yml里配置了master，每次你`hexo g -d` 就会将静态文件部署到该分支，以后无需操心。

创建另外一个分支：
1. 在本地博客根目录 打开git 命令行
2. 把本地博客source commit 到版本区
```
git init 
git add -A
git status 
git commit -m 'blog files'

git remote add hexo git@github.com:lcs1234567/lcs1234567.github.io.git
git push -u hexo  hexo 
```
3. 设置 hexo 为默认分支
    - 点击博客仓库下的setting/Branches
    - 设置默认分支为 hexo
    - 这样以后就默认 clone 该分支

注意，git add -A 时可能目录themes/next 无法被add，这是因为next目录是你clone下来的主题，被另外一个.git/ 管理。我采用简单粗暴方法是在next/ 打开 git bash
```
ls -a
rm -rf .git/
```
4. 在公司设备 git clone 博客仓库，`hexo g -d`前再次生成私公钥，因为私公钥是相对设备的，设备换了必须添加该设备的公钥。

#### 添加评论功能
评论功能可选择gitalk/valine/来必力，我集成的是valine。
1. 在[valine](https://leancloud.cn/dashboard/applist.html#/apps) 注册一个应用，查看app id, app key
2. 修改 themes/next 的主题配置文件_config.yml 
```
valine:
  enable: true
  appid:  [your app id]
  appkey: [your app key]
  notify: false # mail notifier , https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: Just go go # comment box placeholder
  avatar: mm # gravatar style
  guest_info: nick,mail,link # custom comment header
  pageSize: 10 # pagination size
```

参考文章
>1. [hexo 官方文档](https://hexo.io/zh-cn/docs/)
>2. [next 主题配置官方文档](https://theme-next.iissnan.com/getting-started.html)
>3. [集成 valine评论功能](https://www.bluelzy.com/articles/use_valine_for_your_blog.html)
>4. [添加博客主题其他样式](http://shenzekun.cn/hexo%E7%9A%84next%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.html)
>5. [设置博客首页摘要](https://www.jianshu.com/p/393d067dba8d)

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！