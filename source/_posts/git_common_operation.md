title: git 常用操作
date: 2018/9/09
categories:

- 基础
- git
tags:
- git
comments: true
---
Git是流行的分布式项目版本管理系统，利用git flow这个优秀的分支模型可以实现多人合作开发项目；在 githbu上也可以学习其他优秀项目！在这里结合自己使用习惯，总结一下git的常用操作。想详细了解git的开发人员可以参考
> 1. [廖雪峰git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

#### git安装
一般是先 [下载git客户端](https://git-scm.com/downloads)，安装好之后在电脑桌面点击右键，能看到Git Bash Here就说明安装成功！在本地利用 git初始化项目，然后再 push到你的 github上。接下来模拟一下这一过程，
1. 在桌面新建文件夹 project
2. 进入 project，点击右键在此目录下进入 git bash
```bash
# 设置该git user
$ git config --global user.name  "Your Name"
$ git config --global user.email "email@example.com

# 初始化
git init

# 此时 project内有隐藏文件 .git管理，查看
ls -a
```
3. 编辑文件
```bash
# 创建 1.txt 加入一段文字，也可以采用 vim
touch 1.txt
```
4. 利用 `git status`查看项目文件状态，可以显示哪些新增文件，删除文件，或者各个区内的文件状态！

#### git各区概念
git正是利用工作区、暂存区、版本区这三个区来实现分布式管理项目。利用这三个区结合 github就可以实现去中心化，因为每一个开发人员的本地都会有一份项目开发过程的log备份，就算 github挂掉了，只要拿到任何一个开发人员的一份备份也可以还原分布式开发！
1. 工作区，就是在 project目录下看到的文件，git bash编辑命令 `ls -ll` 即可看到工作区的文件！
2. 暂存区，就是 `git add [文件名]`到的目录区，当需提交的文件量多，可以利用 `git add .`添加全部文件，**同时包括删除操作**！执行 add命令，还是显示红色字体，即未添加成功。可能因为某个文件夹内含另外一个被 git管理的目录，删除该目录下隐藏的git 文件即可!
```bash
# 在 project内利用 `git status`看文件状态
# 显示**红色**提示，说明 1.txt未提交到暂存区
git add 1.txt

# 此时 1.txt已提交到暂存区，查看暂存区目录文件
git ls-files
```
3. 版本区，将暂存区的文件提交到版本区内，`git status` 显示的蓝色提示，就是说明添加到暂存区的文件未被提交到版本区，commit至版本区。每次 push就是将版本区的文件提交至 github
```
git commit -m '[本次提交的注释]'
```

#### push项目到 github
当文件成功commit到版本区之后，`git status`显示 nothing to commit，接下来将项目 push到github。
1. [在 githbu上注册账号](https://github.com/)，创建一个仓库
2. 设置提交的远程库，采用 https 协议，origin只是一个名字可以改变
```bash
# 采用https协议，不要配置公私钥，但每次要输密码
git remote add origin https://github.com/lcs1234567/project.git

# -u 下次执行git push 就会默认push origin
git push -u origin master
```
3. 亦可以采用ssh协议，因为ssh协议，要配置设备私公钥至github，待私公钥生成，打开存放目录，将公钥复制，添加到你的github，点击头像/setting/SSH and GPG keys，将项目 push到 github。第一次利用ssh push是会提示 authenticity can't be established，输入yes，回车就可以了！
```bash
# 生成设备私公钥, git bash中输入以下命令，一路按回车键
ssh-keygen -t rsa

git remote add origin git@github.com:lcs1234567/project.git

git push origin master
```
4. 在本地git bash可以查看配置好的远程仓库
```bash
git remote -v
```

#### git 常用命令
```bash
# 设置 本机git user
$ git config --global user.name  "Your Name"
$ git config --global user.email "email@example.com"

# https 协议 和 SSH 协议连接 github 区别


# 删除git 本地分支
$ git branch -d <branch_name>

# 删除github 远程分支
$ git push --delete <remote_name> <branch_name>


# 本地 创建并切换到该分支
 git checkout -b develop

# 拉取远程分支，并且切换到该分支（原先本地没该分支）
git checkout -b <branch_name> <remote_name>/<branch_name>

git checkout -b develop origin/develop
或者
git checkout -t origin/develop


# 查看远程分支
git branch -a


# 在某个分支上回归版本
git log  // 查看历史 commitId

git log --pretty=oneline // 查看当前所在commitId

git reset --hard <commitId>

# git tag 命令
git tag
git tag 0.0.1
git tag -a 0.0.1 -m "version 0.1 released" 
git tag -a 0.0.1 -m "version 0.1 released" 1094adb
git show 0.0.1

# 删除本地tag
git tag -d 0.0.1

# push tag
git push origin 0.0.1
git push origin --tags

# 删除远程tag
git tag -d 0.0.1
git push origin :refs/tags/0.0.1
```

#### [git flow](https://www.cnblogs.com/cnblogsfans/p/5075073.html) 
一个人开发时，master/develop 两个分支就可以满足要求。项目组合作就不同了，会出现各种问题。比如
* 不可能只在develop 开发，今天你在develop 完成需求A，需求B 开发到一半，要求发版本怎么办？
* 从develop 迁出分支完需求，怎样分类、明确分支功能。如feature/bugfix
* 怎样发版本，打tag，线上版本处理bug 怎样快速修复

git flow 该分支模型就是用来解决多人合作开发遇到的问题，[理解 gitflow这篇文章足够了](https://www.cnblogs.com/cnblogsfans/p/5075073.html)

#### git GUI
刚开始我一点也不喜欢git 的图形化界面操作，认为用命令行操作很 cool，但是部门项目开发、测试流程繁琐，并且依赖人工记忆。一般项目环境分为（其中部署可用shell script 做成可视化操作），
1. 本地开发
2. 内网测试
3. 外网部署，可能有个外测

由于在内网测试的功能不会一定要部署到正式环境，因此部门添加了一个pre-develop 分支，完成需求流程就变成这样，
1. develop 迁出 feature，完成需求，合并到pre-develop 
2. 自己将 pre-develop 部署到内网，找验收需求方测试
3. 测试通过，就把feature finish 到 develp，再部署外网。这一步还利用了code review 的步骤，开发人员通过 pull request 提交合并，再由检查你代码写得是否规范的同事完成合并。

如果测试之后有问题需要修改，就必须来回切换分支（流程不能出错）。万一把代码写在错的分支就悲剧了，所以 [git GUI sourcetree](https://www.sourcetreeapp.com/) 能帮我们减轻心智负担，多出时间去完成自己的todo，学习其他技术，锻炼身体，哈哈！

![git1](/images/20180910/git.png)

这就是一些常用的git 管理项目的流程，欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！
