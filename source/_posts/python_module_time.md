title: python datetime模块
date: 2018/9/16
categories:
- python
- 常用模块
tags:
- python
comments: true
---
当大家用到时间属性去分析数据时，难免会对时间做各种的转化；步骤虽然不难，代码一 google也能得到。但如果不明白其中原理，每次搜索得到的代码会比较繁杂。

而且我希望自己写代码能够，思路越来越明朗，代码量越来越少，速度越来越快！因此想总结一下python datetime模块对时间的常用操作。

#### time 一些概念
编程时常遇到一些时间的概念如，epoch/UTC(GMT)/时间戳，这是人们为了把时间表达清楚而下的定义。你想精准把一个时间表示成秒数，需要
1. 基准时间，就是从哪个时间起呢？
2. 时区

我们把1970年1月1日 00:00:00 UTC+00:00时区的时刻称为epoch time，UTC代表的是0时区（UTC+0:00也代表0时区），北京时间在东八区（UTC+8:00）。所以
1. 时间戳表示的就是0时区的现在时间距epoch的秒数
    - 时间戳规定在0时区进行计算，这样就有
    ```
    timestamp = 0 = 1970-1-1 00:00:00 UTC+0:00
    
    timestamp = 0 = 1970-1-1 08:00:00 UTC+8:00
    
    ```
    - 已知时间戳，马上就已知UTC+0:00，再转其他时区
2. 本地当前时间转时间戳
    - 获取操作系统设定的时区
    - 转成UTC+0:00
    - 计算距epoch 的毫秒数
3. 把时间戳转日期
    - 也要先获取时区，再由UTC转过来
    - [datetime里面封装了time/date 模块](https://github.com/python/cpython/blob/3.7/Lib/datetime.py)，很多时候内部指定了时区

#### datetime 常用操作
1. 获取日期
    - 今天日期，先获取datetime类型，再转格式化字符串!
    ```
    from datetime import datetime
    
    datetime.now() 
    out: datetime.datetime(2018, 9, 18, 20, 20, 7, 433321)
    
    
    datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    out: '2018-09-18 20:24:40'
    ```
2. 获取时间戳
    - 今天日期时间戳: 先获取今天日期datetime 类型，再转时间戳
    ```
    datetime.now().timestamp()
    out： 1537273614.038577
    ```
    - 把日期转时间戳: 把日期转datetime类型，再转时间戳
    ```
     datetime.strptime('2018-09-18 20:24:40', '%Y-%m-%d %H:%M:%S')
     out: datetime.datetime(2018, 9, 18, 20, 24, 40)
     
     
     datetime.strptime('2018-09-18 20:24:40', '%Y-%m-%d %H:%M:%S').timestamp()
     out：1537273480.0
    ```
3. 时间戳转日期
    - 有时间戳了，datetime提供了方法转成本地时间
    ```
    datetime.fromtimestamp(1537273480.0)
    out: datetime.datetime(2018, 9, 18, 20, 24, 40)
    
    datetime.fromtimestamp(1537273480.0).strftime('%Y-%m-%d %H:%M:%S')
    out: '2018-09-18 20:24:40'
    
    ```
    - 时间戳转成 UTC时间
    ```
    datetime.utcfromtimestamp(1537273480.0)
    out: datetime.datetime(2018, 9, 18, 12, 24, 40)
    
    datetime.utcfromtimestamp(1537273480.0).strftime('%Y-%m-%d %H:%M:%S')
    out: '2018-09-18 12:24:40'
    ```
4. 时间差
    - 已知一个时间，差距天数，求另外一个日期
    ```
    from datetime import timedelta
    datetime.now() + timedelta(days=1)
    out：datetime.datetime(2018, 9, 19, 21, 2, 9, 887597) 
 
 
    datetime.now() + timedelta(days=1)
    out： datetime.datetime(2018, 9, 17, 21, 2, 23, 40349)
    
    ```
    - 两个时间差的天数：datetime相减即可
    ```
    a = datetime.fromtimestamp(1537273480.0)
    b = datetime.now()
    
    (b-a).days
    (b-a).seconds 
    
    out: 0
    out: 1929
    ```
#### mysql 常用命令
1. mysql 一些常用命令
    - 时间戳转时间
    ```
    select from_unixtime(1534995065);
    out: 2018-08-23 11:31:05
    ```
    - 字符串转时间
    ```
    select str_to_date('2016-01-02', '%Y-%m-%d %H');
    out: 2016-01-02 00:00:00
    ```
    - 时间转时间戳
    ```
    select unix_timestamp(now());

    out：1452001082
    ```
参考文章
>1. [datetime — Basic date and time types](https://docs.python.org/3/library/datetime.html)

这就是一些关于时间的常用操作，欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！