title: sql 常用
date: 2019/01/12
categories:
- web
- sql
tags:
- sql
comments: true
---

sql 常用理解

#### groupby / having
sql 分组查询使用格式
```sql
# A/B 是分组的列，f 为聚合函数，having 后面跟聚合函数
select A, B, f(C) from table where [xx] groupby A, B having xxx
```

sql 语句先执行条件筛选where，再根据A/B 分组，分组之后的中间结果相当于下面那样这样，再执行having、select。所以select、having 后面不能直接选择分组以外的列。但是表里面根本不存在以下形式，只是便于理解才这样写的！
```mysql
ColumA = A1, A2 A3
groupby A
---------
A1 c1 d1
   c1 d3
   c6 d89

A2 c3 d4
   c7 d7

A3 c6 d7
   c8 d9
   c1 d9
   c0 d78


select A sum(C)
------
A1 c1+c1+c6
A2 c3+c7
A3 c6+c8+c1+c0

# 假设选择 select A，C  数据库就不知道怎样得出结果
```

lavarel 查询构造器实现分组
```php
# laravel 查询构造器， Eloquent ORM语法
 self::selectRaw('mtime, count(*) as retention_role_total')
        ->where($where)->groupBy('mtime')
        ->get()->toArray();
```
#### join
数据库连接的几种用法，在下面表数据测试，用法举例
* 内连接，包括等值连接，不等值连接，自然连连接
* 外连接，左连接，右连接，全连接
* 交叉连接
```mysql
mysql> select * from game;  
+-----+--------------+----------+
| gid | gameName     | playerId |
+-----+--------------+----------+
|   1 | 王者荣耀      |        1 |
|   2 | 吃鸡游戏      |        2 |
|   3 | 月圆之夜      |        3 |
|   4 | 元气骑士      |        4 |
+-----+--------------+----------+

mysql> select * from player;
+----------+------------+-----+
| playerId | playerName | gid |
+----------+------------+-----+
|        1 | playerA    |   1 |
|        2 | playerB    |   2 |
|        5 | playerE    |   5 |
+----------+------------+-----+
```
内连接
```mysql
select * from game as a inner join player as b on a.playerId = b.playerId 
+-----+--------------+----------+----------+------------+-----+
| gid | gameName     | playerId | playerId | playerName | gid |
+-----+--------------+----------+----------+------------+-----+
|   1 | 王者荣耀      |        1 |        1 | playerA    |   1 |
|   2 | 吃鸡游戏      |        2 |        2 | playerB    |   2 |
+-----+--------------+----------+----------+------------+-----+
```
左连接
```mysql
select * from game as a left join player as b on a.playerId = b.playerId
+-----+--------------+----------+----------+------------+------+
| gid | gameName     | playerId | playerId | playerName | gid  |
+-----+--------------+----------+----------+------------+------+
|   1 | 王者荣耀     |        1 |        1 | playerA    |    1 |
|   2 | 吃鸡游戏     |        2 |        2 | playerB    |    2 |
|   3 | 月圆之夜     |        3 |     NULL | NULL       | NULL |
|   4 | 元气骑士     |        4 |     NULL | NULL       | NULL |
+-----+--------------+----------+----------+------------+------+
```
右连接
```mysql
select * from game as a right join player as b on a.playerId = b.playerId
+------+--------------+----------+----------+------------+-----+
| gid  | gameName     | playerId | playerId | playerName | gid |
+------+--------------+----------+----------+------------+-----+
|    1 | 王者荣耀      |        1 |        1 | playerA    |   1 |
|    2 | 吃鸡游戏      |        2 |        2 | playerB    |   2 |
| NULL | NULL         |     NULL |        5 | playerE    |   5 |
+------+--------------+----------+----------+------------+-----+
```
全连接
```sql
# mysql 里面没有full join
select * from game as a full outer join player as b on a.playerId = b.playerId
```
交叉连接
```mysql
select * from game as a cross join player as b order by a.playerId;
+-----+--------------+----------+----------+------------+-----+
| gid | gameName     | playerId | playerId | playerName | gid |
+-----+--------------+----------+----------+------------+-----+
|   1 | 王者荣耀      |        1 |        1 | playerA    |   1 |
|   1 | 王者荣耀      |        1 |        2 | playerB    |   2 |
|   1 | 王者荣耀      |        1 |        5 | playerE    |   5 |
|   2 | 吃鸡游戏      |        2 |        2 | playerB    |   2 |
|   2 | 吃鸡游戏      |        2 |        5 | playerE    |   5 |
|   2 | 吃鸡游戏      |        2 |        1 | playerA    |   1 |
|   3 | 月圆之夜      |        3 |        1 | playerA    |   1 |
|   3 | 月圆之夜      |        3 |        2 | playerB    |   2 |
|   3 | 月圆之夜      |        3 |        5 | playerE    |   5 |
|   4 | 元气骑士      |        4 |        1 | playerA    |   1 |
|   4 | 元气骑士      |        4 |        2 | playerB    |   2 |
|   4 | 元气骑士      |        4 |        5 | playerE    |   5 |
+-----+--------------+----------+----------+------------+-----+
```

laravel 实现等值连接
```php
public static  function getData($params, $page, $pageSize){
    $joinWhere = [
        ['a.game_id', '=', $params['pack']
    ];
    $offset = ($page - 1) * $pageSize;
        
    $where = [];    
    self::selectRaw('a.ad_id, b.ad_platform')
         ->join('b', function($join) use ($joinWhere){
            $join->on('a.ad_id', '=', 'b.ad_id')
                 ->where($joinWhere);
         })
        ->where($where)
        ->offset($offset)->limit($pageSize)
        ->get()->toArray();
}
```

#### 索引

#### 数据库权限

欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！