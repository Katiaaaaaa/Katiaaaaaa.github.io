---
title: MySQL题库(6)
date: 2020-10-09
tags: ["数据库","数据分析","MySQL-联合子查询","MySQL-左连接","MySQL-子查询"]
categories: ["MySQL"]
author: "Katia"
---

> * 多个条件并行子查询
> * 分组聚合+条件统计

<!--more-->

## 1158. 市场分析 I

Table: Users


| Column Name    | Type    |
|  :-------: | :-----------:|
| user_id        | int     |
| join_date      | date    |
| favorite_brand | varchar |

此表主键是 user_id，表中描述了购物网站的用户信息，用户可以在此网站上进行商品买卖。

Table: Orders


| Column Name   | Type    |
|  :-------: | :-----------:|
| order_id      | int     |
| order_date    | date    |
| item_id       | int     |
| buyer_id      | int     |
| seller_id     | int     |

此表主键是 order_id，外键是 item_id 和（buyer_id，seller_id）。

Table: Item


| Column Name   | Type    |
|  :-------: | :-----------:|
| item_id       | int     |
| item_brand    | varchar |

此表主键是 item_id。
 

请写出一条SQL语句以查询每个用户的注册日期和在 2019 年作为买家的订单总数。

查询结果格式如下：

Users table:

| user_id | join_date  | favorite_brand |
|  :-------: | :-----------:|:-----------:|
| 1       | 2018-01-01 | Lenovo         |
| 2       | 2018-02-09 | Samsung        |
| 3       | 2018-01-19 | LG             |
| 4       | 2018-05-21 | HP             |


Orders table:

| order_id | order_date | item_id | buyer_id | seller_id |
|  :-------: | :-----------:|:-----------:|:-----------:|:-----------:|
| 1        | 2019-08-01 | 4       | 1        | 2         |
| 2        | 2018-08-02 | 2       | 1        | 3         |
| 3        | 2019-08-03 | 3       | 2        | 3         |
| 4        | 2018-08-04 | 1       | 4        | 2         |
| 5        | 2018-08-04 | 1       | 3        | 4         |
| 6        | 2019-08-05 | 2       | 2        | 4         |


Items table:

| item_id | item_brand |
|  :-------:|:-----------:|
| 1       | Samsung    |
| 2       | Lenovo     |
| 3       | LG         |
| 4       | HP         |


Result table:

| buyer_id  | join_date  | orders_in_2019 |
|  :-------: | :-----------:|:-----------:|
| 1         | 2018-01-01 | 1              |
| 2         | 2018-02-09 | 2              |
| 3         | 2018-01-19 | 0              |


来源：力扣（LeetCode）



```sql
# 统计为0 的也显示出来 使用左联结 ifnull
select u.user_id as buyer_id, u.join_date, ifnull(a.orders_in_2019,0) orders_in_2019 from 
users u left join 

(select buyer_id, count(order_id) as orders_in_2019 from users u 
inner join orders o on u.user_id = o.buyer_id 
where order_date between '2019-01-01' and '2019-12-30'
group by buyer_id order by buyer_id)a 

on u.user_id =  a.buyer_id;
```