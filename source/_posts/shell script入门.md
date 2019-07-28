title: shell script 入门
date: 2018/12/15
categories:

- 基础
- linux
tags:
- shell script
comments: true
---

#### 为什么学shell
操作系统其实是一组软件，控制整个计算机硬件如cpu调度，内存、磁盘存储输入输出是操作系统的内核（Kernel）。通常这组软件是不允许被没有管理能力的终端用户随便使用的，但总是需要让用户操作系统的，用户可以通过壳程序（shell）来达到目的。**壳程序是提供给用户去操作系统的一个接口**，像ls、mkdir这些常用的命令都是封装好的独立应用程序，它们可以**利用壳程序再通过内核**去操作系统。

linux 很多地方都利用了shell script，shell 在系统管理是很好的工具，服务的启动关闭就是通过它来控制的。所以学习shell 有助于弄清楚linux的来龙去脉，是进一步理解linux的方式。

但是shell script 不擅长大量数值运算，它调用外部函数库耗cpu。linux 默认的shell 是bash，它规范了接口的语法。学习shell 可以参考
1. [认识和学习bash](http://cn.linux.vbird.org/linux_basic/0320bash.php#variable)
2. [学习shell scripts](http://cn.linux.vbird.org/linux_basic/0340bashshell-scripts.php#script)

#### shell 操作环境
利用系统提供的命令如，ls/mkdir/rm/mv 等，或者通过./xx.sh 执行已写好的可执行脚本。必须先搞清楚系统**查询执行这些命令的顺序**，所以在当前目录执行脚本文件时要加 ./ 代表相对，否则报找不到文件错误
1. 以相对/绝对路径运行命令，例如『 /bin/ls 』或『 ./ls 』；
2. 由 alias 找到该命令来运行；
3. 由 bash 内建的 (builtin) 命令来运行；
4. 透过 $PATH 这个变量的顺序搜寻到的第一个命令来运行。

我们一登入linux 进入shell 之后，能使用一堆有用的变量，这是因为系统有一些环境配置文件的存在。**bash 在启动时直接就读取这些配置文件，以规划好bash 的操作环境**。配置文件有分全体系统的配置文件以及用户个人偏好设置文件。
*  /etc/profile 系统整体的配置
*  ~/.bash_profile  ~/.bash_login ~/.profile 属于使用者个人配置

同时也必须理解login shell 和 non-login shell 的概念，因为通过这两种进入的bash 环境，读取配置文件的顺序不同。
* login shell，**取得bash 需要完整的登录流程**，比如通过secureCRT ssh 方式得到的bash 环境。
* non-login shell，**取得bash 接口的方法不需要重复的登陆举动**，比如你在原本的bash 环境再次下达bash 命令，没有输入账号密码，那第二个bash 子程序也是non-login shell。所以在**写脚本命令前一般都会配置环境变量**

下面是这两种方式读取配置文件的顺序，图来自鸟哥书籍。实线是主线流程，虚线是被调用的配置文件。图中是**login shell 完整顺序图，而non-login shell 只读 ~/.bashrc**

![duck1](/images/20181215/bash1.png)

#### 简单 shell script
下面是简单shell script 的格式，**PATH 很重要**
```
#!/bin/bash
# Program:
#       This program is a easy shell script
# History:
# 2019/07/23    lics    First release
PATH=/usr/local/php/bin:/usr/local/node/bin:/usr/local
export PATH

# 文件名
date=$(date +%Y%m%d)
file=${date}

# file 加不加{}只是习惯问题
touch "$file"
```

#### shell script 运行区别
* sh script 或者 ./script ，利用直接运行的方式运行script，**该script 都会使用一个新的bash 环境来运行脚本内的命令**。script 是指子程序的bash 内运行的！注意的是，在子程序的各项变量或者动作将会结束而不会回到父程序中。
* source script ，在父程序中运行脚本，**script 的各项动作都会在父程序中生效**。所以我们修改了/etc/profile 里面的环境变量时，都会**运行source /etc/profile 使其有效**，否则要重新登入才行。
* exec 方式

#### bash 语法
在了解shell 的作用，操作环境，运行方式之后，可以进一步学习bash的基本语法。比如
* bash 变量
* [bash 通配符和特殊符号](http://cn.linux.vbird.org/linux_basic/0320bash.php#settings_wildcard)
   * ()代表中间为**子shell 的起始和结束**
* 判断式，关于文件类型，文件权限，整数，字符串
* [shell script 的默认变量 $0，$1](http://cn.linux.vbird.org/linux_basic/0340bashshell-scripts.php#dis3)
   * $# ：代表后接的参数『个数』，以上表为例这里显示为『 4 』；
   * $@ ：代表『 "$1" "$2" "$3" "$4" 』之意
   * $* ：代表『 "$1c$2c$3c$4" 』，其中 c 为分隔字节，默认为空白键 
* 条件判断式，if--then/case---esac/function
* 回圈，while--do/for----do

#### 定时服务器脚本
通过一些例子来熟悉shell script， 写一个定时同步服务器时间的 script
1. 和网络时间同步
```bash
# 安装ntpdate工具
yum -y install ntp ntpdate

# 设置系统时间与网络时间同步
ntpdate cn.pool.ntp.org

# 将系统时间写入硬件时间
hwclock --systohc
```
2. **定时同步，利用crontab 来执行定时脚本**
```
# 编辑crontab
vim /etc/crontab

# 添加定时任务，每6分钟同步一次
*/10 * * * * ntpdate -u ntp.api.bz && clock -w

service crond reload
```
#### 自动连接mysql
每次都要通过输入密码连接数据库，难免烦躁，可以把数据库密码放在每个文件中，然后写script 连接
```bash
# 创建con_mysql脚本文件，密码在/data/save/mysql_root 内
exec mysql -uroot -p$(cat /data/save/mysql_root) 

# 把con_mysql 所在目录添置PATH，在任何目录执行
con_mysql 
```

#### 数据自动入库
假如现在有很多.sql 和txt 文件数据要录入数据库中，我们可以写一个只要提供数据库名、文件所在目录名就可以完成目的的shell script。**运行方式 [script] db_name  [sql所在目录绝对路径]** 
```bash
#!/bin/bash
database=$1
backup_dir=$2

if [ "$database" == "" -o "$backup_dir" == ""  ] ; then
  echo "eg:bash $0 db_name data_dir"
  exit
fi

mysql -uroot -p`cat /data/save/mysql_root` -e "CREATE DATABASE IF NOT EXISTS ${database};"
cd ${backup_dir}
SQL_LIST=$(ls ${backup_dir} | /bin/grep sql)
for sql_file in ${SQL_LIST}; do
    mysql -uroot -p`cat /data/save/mysql_root` ${database} < ${sql_file}
done
mysqlimport --local -uroot -p`cat /data/save/mysql_root` ${database} *.txt
```

#### script 更新项目版本
在项目开发中，项目版本更新过程一般如下
1. 本地环境完成，自测无误
2. 将代码push github 
3. 在内测机器pull 项目，进行测试

在我们push 仓库后，每次都要手动去更新内测机器的代码，难免烦躁。可以写一个可视化操作界面，只需点击一个按钮，利用**shell script 来完成git pull 和更新到内测机器相应项目目录的步骤**


欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！