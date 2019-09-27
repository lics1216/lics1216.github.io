title: sql 常用
date: 2019/01/12
categories:
- web
- sql
tags:
- sql
comments: true
---

sql 常用

#### groupby / having
sql 分组查询使用格式为，
```
# 注意 select 不能出现没有分组的列，只能用聚合函数
# having 后面也是跟聚合函数，操作单位是每个分组 having count(xx) > 2
select A, B, f(C) from table where [xx] groupby A, B having xxx
```

分组的中间结果相当于这样，但表里面根本不存在以下形式
```
A = A1, A2 A3

A1 x1 y1
   x1 y3
   x6 y89

A2 x3 y4
   x7 y7

A3 x6 y7
   x8 79
   x1 y9
   x0 y78
```

lavarel 查询构造器实现分组
```
 DB::connection($this->dbConnect)->table($this->table)
        ->selectRaw('mtime, count(*) as retention_role_total')
        ->where($where)->groupBy('mtime')
        ->get();
```
#### join

#### 索引

#### 数据库权限

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！