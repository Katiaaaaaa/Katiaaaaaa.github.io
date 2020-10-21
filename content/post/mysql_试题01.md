  
---
title: MySQL题库(1)
date: 2020-09-10
tags: ["数据库","数据分析","MySQL-联合子查询","MySQL-左连接","MySQL-子查询"]
categories: ["MySQL"]
author: "Katia"
---

> * 多条件子查询
> * 聚合函数条件前置
> * 分组取最值

<!--more-->

配送表: Delivery

| Column Name                 | Type    |
|  :-------: | :-----------:|
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |

如果顾客期望的配送日期和下单日期相同，则该订单称为 「即时订单」，否则称为「计划订单」。
「首次订单」是顾客最早创建的订单。我们保证一个顾客只会有一个「首次订单」。

写一条 SQL 查询语句获取即时订单在所有用户的首次订单中的比例。保留两位小数。


| delivery_id | customer_id | order_date | customer_pref_delivery_date |
|  :-------: | :-----------:| :-------:  | :-------: | 
| 1           | 1           | 2019-08-01 | 2019-08-02                  |
| 2           | 2           | 2019-08-02 | 2019-08-02                  |
| 3           | 1           | 2019-08-11 | 2019-08-12                  |
| 4           | 3           | 2019-08-24 | 2019-08-24                  |
| 5           | 3           | 2019-08-21 | 2019-08-22                  |
| 6           | 2           | 2019-08-11 | 2019-08-13                  |
| 7           | 4           | 2019-08-09 | 2019-08-09                  |



Result 表：

| immediate_percentage |
|  :-------: | 
| 50.00                |


1 号顾客的 1 号订单是首次订单，并且是计划订单。  
2 号顾客的 2 号订单是首次订单，并且是即时订单。  
3 号顾客的 5 号订单是首次订单，并且是计划订单。  
4 号顾客的 7 号订单是首次订单，并且是即时订单。  
因此，一半顾客的首次订单是即时的。  



思路：
* 计算首次订单作为分母
* 在首次订单基础上添加条件-订单配送日期相等筛选出即时订单作为分子
* 将分母条件和分子条件左连接后 计算得出结果
```sql
SELECT round(COUNT(a2.delivery_id) / COUNT(a1.delivery_id) * 100, 2) AS immediate_percentage
FROM (
  SELECT delivery_id
  FROM delivery
  WHERE (order_date, customer_id) IN (
    SELECT MIN(order_date), customer_id
    FROM delivery
    GROUP BY customer_id
  )
) a1
  LEFT JOIN (
    SELECT delivery_id, order_date
    FROM delivery
    WHERE customer_pref_delivery_date = order_date
  ) a2
  ON a1.delivery_id = a2.delivery_id;
```
优化思路：

由于即时订单是在首次订单的基础上进行筛选         也就是说两者有共同的筛选条件
将其作为where从句部分
分子计算直接用sum函数加上条件
用到虚拟表+子查询
```sql
  select round (
    sum(order_date = customer_pref_delivery_date) * 100 /count(*),2
) as immediate_percentage

from Delivery

where (customer_id, order_date) in 

(
    select customer_id, min(order_date)
    from delivery
    group by customer_id
)
```


来源：力扣（LeetCode）