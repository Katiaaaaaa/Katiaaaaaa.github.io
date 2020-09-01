---
title: MySQL(6)--select执行语句顺序
date: 2020-09-01
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---


> * select-> from-> where-> group by-> max()-> having-> order by
<!--more-->
```sql
select name, max(score) as max_score 
from grade
where name is not null
group by name
having max(score)>600
order by max_score;
```
**select-> from-> where-> group by-> max()-> having-> order by**
* 首先执行from子句,从grade表的数据源
* 执行where语句，筛选grade表中不为null的数据
* 执行group by 语句，把grade表按name分组  
**(此处才可以使用select中的别名)**
* 计算max()聚集函数，从score中分组筛选最大数值
* 执行having子句，筛选score大于600分的数据
* 执行order by 子句，结果按照max_score列排序