title: VMware 安装centos6.5 虚拟机
date: 2018/10/20
categories:

- 基础
- linux
tags:
- vmware
comments: true
---

在本地安装虚拟机，并用secureCRT 连接可以搭建测试环境，提高开发效率。

#### 安装

在vmware 安装centos6.5， 可以选择最小化安装，这种模式好像没有鼠标，但是可以利用vmware 完成粘贴复制，连接好secureCRT后就没影响了。安装了desktop 版本，也可以删除桌面。
-  vmware
-  [centos6.5 bin DVD1](<http://mirror.nsc.liu.se/centos-store/6.5/isos/x86_64/>)

#### secureCRT 连接

[安装 secureCRT，提取码：q9wb](https://pan.baidu.com/s/1sfA3-9WxuR7f-aRJGQtntw)，设置好自己习惯的操作方式，如选中复制，右键粘贴，背景色等。连接
1. 生成私秘钥，Tools-->Create Public Key --> key type: RSA ---> OpenSSH Key format
2. centos 生成私秘钥文件
```bash
# root 用户，/root/.ssh 下生成
ssh-keygen -t rsa 
```
3. 公钥复制到 /root/.ssh/authorized_keys  文件中，**必须是这个文件名**
4. centos 查看ip 地址
```bash
# 设置网卡随系统启动生效
cd /etc/sysconfig/network-scripts
ls //查看网卡文件 如ifcfg-eth0
vim ifcfg-eth0  //修改为 ONBOOT=yes

# 必须 root用户， 重启网卡
service network restart

# 是否生效
ping www.baidu.com  // 没问题

# 查看网卡 ip 
ifconfig  // 如，inet addr:192.168.36.100

# 该ip, 和主机是同一个子网内，
# 在cmd 命令行 ipconfig 查看到 VMware Network Adapter VMnet1
# 因为vmware 默认配置一个网卡ANT 连接方式，没有的话，可能是在网络共享中心--> 更改适配器设置 禁用了
```
5. 连接，File--> Quick Connect ，把publicKey 放到第一位置，连接成功
6. [对网络概念不熟悉可以，参考理解](https://blog.51cto.com/zhoutao/93629)

#### 配置静态ip
上面虚拟机的ip 下次开机可能会发生改变，比如你在vmware 再建立一台虚拟机发现ip 变了。这种情况可以把 ip配置成静态ip
1. vmware 给该虚拟配置网卡（默认就是一张网卡NAT 方式，也可以）
2. VMware -->编辑-->虚拟网络编辑器--> NAT --> 取消dhcp 动态分配
3. VMware -->编辑-->虚拟网络编辑器--> NAT --> NAT 设置，查看网关
4. 配置centos /etc/sysconfig/network-scripts/ifxxx 网卡
```bash
# device 和网卡名一样
# 添加最后5 项就可以了，改成你虚拟机相应ip

DEVICE=eth0
HWADDR=00:0C:29:32:46:15
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.36.100
NETMASK=255.255.255.0
GATEWAY=192.168.36.2
DNS1=192.168.36.2  # 和网关一样 
```
5. root 重启 网卡服务,  service network restart
6. ifconfig  查看是否为自己设置的ip



欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！