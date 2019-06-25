title: linux 入门
date: 2018/10/16
categories:

- 基础
- linux
tags:
- linux
comments: true
---

linux 是后端项目运行的环境，很多开发者都是在windows 下编写代码，后部署到linux 上运行。所以linux 是很重要的基础，是开发人员需要了解的。学习资料参考
> 1. [建议用centos---下载 bin-DVD1](http://mirror.nsc.liu.se/centos-store/6.5/isos/x86_64/)
> 2. [鸟哥私房菜第三版](http://cn.linux.vbird.org/linux_basic/linux_basic.php)
> 3. [安装可参考](/2018/10/20/vmware%20安装centos%20虚拟机/#more)

#### linux 起源
概念性了解，零散的笔记
1. Unix、GUN、Linux起源， 追求：先求有且能运行，再求进一步改善
2. linux 的版本是指内核版本。linux 版本分为两类，一种使用RPM 方式安装软件系统，包括Red Hat、Fedora、SuSE、CentOS; 另外一种 dpkg 方式安装软件，包括 Debian、Ubuntu、B2D
3. linux 多用户、多任务，与Windows系统不同。文件的属性可分为可读，可写，可执行，属性可分为文件拥有者、文件所属用户组、其他非拥有者与用户组者。
4. 缺点，没有特定的支持厂商，游戏支持度不够、专业软件的支持度不够（如：绘图软件）

#### 文件权限
1. linux 是多人多任务的， 用户的配置信息在etc/xx 下面
2. [进入文件夹要x 权限，r 只能查看文件夹的文件列表](http://cn.linux.vbird.org/linux_basic/0210filepermission_2.php)
   * 添加用户，组
3. linux 下面全部都是文件，文件没有后缀，也有一些常见的 .sh、 .tar.gz ...
4. [linux 各个目录存放的规范](http://cn.linux.vbird.org/linux_basic/0210filepermission_3.php)
   * 安装软件一般放在，usr/local 或者 opt

#### 目录规范和操作
1. 查看 环境变量，相对、绝对目录
   * echo PATH
   * 执行当前目录的命令时，加入./
3. [目录与文件常用命令](http://cn.linux.vbird.org/linux_basic/0220filemanager_2.php)
   * touch/mkdir
   * cat
   * vim
   * mv
   * cp/cp -r
   * scp
   * rm/rm -r/rm dir
4. 目录、文件权限，umask, 可以通过umask 提前设定要创建目录的权限值，文件默认权限 666，目录默认权限 777
```bash
    umask 
    umask -S
    umask 022  // 022 的值解释
    # 022 代表0 相对默认权限减去 0 + 0 + 0 权限
    # 2 代表减去 0 + 2 + 0  写权限没有了
```
5. [修改用户权限---chmod/chown/chgrp](http://cn.linux.vbird.org/linux_basic/0210filepermission_2.php)
6. 添加用户、组、other

#### 文件的查找
1. which 在当前环境变量下路径查找
2. whereis 寻找特定文件，速度快
3. find 硬盘查找，速度慢

#### 文件的压缩与打包
压缩，文件用0，1 存储后还有很多空间未填满或者有很多重复数据，将这些空间填满或者减少空间就是压缩技术的目的。
1. 比如数字1，由于 1byte = 8bits，其他7bits 默认为0，第一位为1
2. 比如重复有100 个1, 并不需要真正存储100个1，标记100个1，减少空间

linux 的命令只能压缩一个文件，所以先把文件打包在进行压缩。最常用打包、压缩命令：
* 压缩：gzip -v 文件名
```
 -c  ：将压缩的数据输出到萤幕上，可透过数据流重导向来处理；
 -d  ：解压缩的参数；
 -t  ：可以用来检验一个压缩档的一致性～看看文件有无错误；
 -v  ：可以显示出原文件/压缩文件的压缩比等资讯；
 -#  ：压缩等级，-1 最快，但是压缩比最差、-9 最慢，但是压缩比最好！默认是 -6
```
* [打包：tar](http://cn.linux.vbird.org/linux_basic/0240tarcompress_3.php)
```
tar -zcv -f filename.tar.gz 要被压缩的文件或目录名称
tar -zxv -f filename.tar.gz -C 想解压到的目录
tar -ztv -f filename.tar.gz 查看文件
```
* tar.gz 先打包再压缩的文件

#### linux 正确的关机方法
```bash
# 机器状态 谁在线
who 

# 关机，重启
shutdown -h now // 时间参数务必加，否则跳到单人维护的登入情况
reboot 
sync //把内存数据写到硬盘，上面两个命令包含了这个
```

#### [**安装软件**](https://blog.51cto.com/zero01/1972444)
1. [yum install 原理](http://www.firefoxbug.com/index.php/archives/2777/)
2. **yum install 安装rpm 软件包**
```bash
# 列出可用的 rpm 包
yum list | grep 包名

yum rearch 包名


# 安装
yum install [y] xx // 回答所有的问题 yes

# 删除
yum remove xx

# yum 其他常用命令
ls /etc/yum.repos.d //yum 仓库的配置文件

<!-- 比如卸载桌面 -->
yum grouplist | grep Desktop
yum groupremove -y Desktop

// 修改 /etc/inittab 里面的 为3，不是5 (x11)
# Default runlevel. The runlevels used are:
#   0 - halt (Do NOT set initdefault to this)
#   1 - Single user mode
#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
#   3 - Full multiuser mode
#   4 - unused
#   5 - X11
#   6 - reboot (Do NOT set initdefault to this)
# 
id:3:initdefault:
```
3. **直接安装 rpm 包**
```bash
# 查询是否安装
rpm -q 包名 

# 安装
rpm -ivh 包文件

# 卸载
rpm -e 包名

# 查询 rpm包，a--all   q--query
rpm -aq | grep rpm 


# rpm 其他常用命令
rpm -Uvh 包名 // 包升级
rpm -qi 包名 // 查询指定包信息
rpm -ql 包名 // 列出包安装文件
rpm -qf 文件绝对路径 //查看该文件由那个包安装
```

#### 文本处理技巧
1. vim
2. tail
```
# 跟踪打印输出文件
tailf xx.log

# 倒数10行
tail -10 xx.log
```
3. grep 用于文本内容的查找
```
ls -lh xx | grep xx

ps aux | grep mysql

netstat -tlnp | grep redis
```
4. [sed 用于文本内容的编辑，常用于文本替换]()
```bash
# sed [option] 'command' filename

# option
-n：使用安静(silent)模式。
    在一般sed的用法中，所有来自stdin的数据一般都会被列出到终端上。
    但如果加上-n参数后，则只有经过sed特殊处理的那一行(或者动作)才会被列出来。
-e：直接在命令列模式上进行sed的动作编辑。
-i：直接修改读取的文件内容，而不是输出到终端。直接修改真实文本

# command
a：追加，a的后面可以接字串，而这些字串会在新的一行出现(目前的下一行)
i：插入，i的后面可以接字串，而这些字串会在新的一行出现(目前的上一行)
d：以行为单位的删除
c：以行为单位的替换，c的后面可以接字串
s：在行中搜寻并替换
p：以行为单位的显示，通常p会与参数sed -n一起运行

# 比如
```
5. [awk 用于对文本进行分析](http://www.ruanyifeng.com/blog/2018/11/awk.html)，当要对处理的数据生成报告，或者数据是按照列进行处理时，使用awk 更方便
```bash
# 输出第一列 
awk '{print $0}' demo.txt

# 用冒号分割每行文本 
awk -F ':' '{ print $1 }' demo.txt

# ....
```
6. 数据流重定向
```bash
# 将ls -l 输出的信息重定向到 /data/tmp/rootfile
ls -l > /data/tmp/rootfile

# 追加
ls-l >> /data/tmp/rootfile

# cat 创建文件
cat > catfile 
xx
xx
按ctrl+d 离开

# 输入 eof 结束
cat > catfile << 'eof'
xx
xx
eof 

# 用文件代替手动输入
cat > catfile < filename
```

#### shell script
[学习shell script](http://cn.linux.vbird.org/linux_basic/0340bashshell-scripts.php)
1. shell script有助于弄清楚 linux的来龙去脉，linux 很多处利用shell script
2. shell script 用在系统管理很好的工具，但不擅长大量数值运算上（调用外部函数库，耗cpu）

shell script文件运行须知
1. xx.sh 文件具备rx 权限
```
# 命令运行顺序（重要）
1. 以相对/绝对路径运行命令，例如『 /bin/ls 』或『 ./ls 』；
2. 由 alias 找到该命令来运行；
3. 由 bash 内建的 (builtin) 命令来运行；
4. 透过 $PATH 这个变量的顺序搜寻到的第一个命令来运行。
```
2. bash 是shell 的一种，所以script 第一行必须写明利用那种shell
```
#!/bin/bash
# Program:
#       This program shows "Hello World!" in your screen.
# History:
# 2019/06/24	lcs	First release
```
3. [变量使用](http://cn.linux.vbird.org/linux_basic/0320bash.php#variable)
4. 写一个连接 mysql 的脚本文件
```
# 连接 mysql
exec mysql -uroot -p$(cat /data/save/mysql_root) "$@"

# 三种shell脚本调用方法(fork, exec, source) ?
```

#### 查看进程和端口号
[ps aux 查看运行程序](http://cn.linux.vbird.org/linux_basic/0440processcontrol_3.php#ps)

```bash
# a 不与 terminal 有关的所有 process 
# u 有效使用者 (effective user) 相关的 process 
# x 通常与 a 这个参数一起使用，可列出较完整资讯。

ps aux

# USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.0  0.0  19232  1508 ?        Ss   18:55   0:03 /sbin/init

# user process 属于的用户 
# PID  识别码
# %CPU 使用的CPU 资源占比
# %MEM 内存百分比
```
[netstat -tlnp 网络联机状态](http://cn.linux.vbird.org/linux_basic/0440processcontrol_3.php#netstat)
```bash
# -a  ：将目前系统上所有的连线、监听、Socket 数据都列出来
# -t  ：列出 tcp 网络封包的数据
# -u  ：列出 udp 网络封包的数据
# -n  ：不以程序的服务名称，以埠号 (port number) 来显示；
# -l  ：列出目前正在网络监听 (listen) 的服务；
# -p  ：列出该网络服务的程序 PID 

netstat -tlnp

# Proto Recv-Q Send-Q Local Address             Foreign Address             State       PID/Program name   
tcp        0      0 0.0.0.0:80                  0.0.0.0:*                   LISTEN      1996/nginx
```

#### [linux 启动流程](http://cn.linux.vbird.org/linux_basic/0510osloader_1.php)
理解linux启动时都做什么可以完成一些设置，比如完成 卸载桌面
```
<!-- 比如下载桌面 -->
yum grouplist | grep Desktop
yum groupremove -y Desktop

// 修改 /etc/inittab 里面的 为3，不是5 (x11)，如下：
# Default runlevel. The runlevels used are:
#   0 - halt (Do NOT set initdefault to this)
#   1 - Single user mode
#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
#   3 - Full multiuser mode
#   4 - unused
#   5 - X11
#   6 - reboot (Do NOT set initdefault to this)
# 
id:3:initdefault:
```
比如，设置服务在开机时自启动
```
# 在 /etc/rc.d/rc.local 添加 

/etc/init.d/mysql start
/etc/init.d/redis start
/etc/init.d/php7-fpm start
```

#### 同步网络时间
```
# 安装ntpdate工具
yum -y install ntp ntpdate

# 设置系统时间与网络时间同步
ntpdate cn.pool.ntp.org

# 将系统时间写入硬件时间
hwclock --systohc

# 开机启动校对
echo "ntpdate -u ntp.api.bz && clock -w" >> /etc/profile
```

#### 用户群主概念、账号管理
1. UID/GID，比如用户 lcs，在/etc/passwd 里面对应 lcs---UID 500
```
lcs:x:500:500:lcs:/home/lcs:/bin/bash

<!--各个字段含义：-->
1. lcs 名词
2. x 口令，但是未显示，存在/etc/shadow
3. 500 UID 0代表root，1-499 系统账号
4. 500 GID
5. lcs 用户信息说明栏
6. /home/lcs 家目录 
7. /bin/bash 给用户一个shell（bash） 和系统沟通
```
2. /etc/shadow 存放各用户密码
3. lcs 用户一登入就有一个初始群组lcs，lcs用户可以加入其它群组，加入这么多群组，创建新文件属于的群组是有效群组。
```
<!--组名和GID 对应-->
etc/group

root:x:0:root
1. root 群组名
2. x 口令，未显示
3. 0 GID
4. root 该群组包含的用户，把lcs 加入会显示 root,lcs

<!--查看所属群组，第一个为有效群组，新文件所属群组-->
groups

```
4. 可以切换lcs 用户的有效群组，涉及/etc/gshadow
5. 账号管理概念和实践
   * useradd lcs
   * passwd lcs

#### 扩充linux 根目录 
下面链接都可以完成扩容，计算机里面一个磁盘显示的是 /dev/sda，两个显示/dev/sdb，利用 fdisk对磁盘进行分区。理解 pv/vg/lv 对完成扩容有帮助
1. [扩容--详细](https://blog.51cto.com/woyaoxuelinux/1973718)
2. [扩容--简略](https://blog.csdn.net/Hu_wen/article/details/60141347)
3. [pv/vg/lv 概念理解](https://www.linuxidc.com/Linux/2017-05/143724.htm)
```
/dev/sda1
/dev/sda2 ...

<!-- 删除未知lv -->
vgreduce --removemissing [vgName]
vgreduce --help
```

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！