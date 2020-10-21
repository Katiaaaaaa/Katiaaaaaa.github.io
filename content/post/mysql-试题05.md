---
title: MySQL题库(5)
date: 2020-10-09
tags: ["数据库","数据分析","MySQL-union","MySQL-all"]
categories: ["MySQL"]
author: "Katia"
---

> * 左连接
> * 两次分组

<!--more-->

编写一条 SQL 查询来找出在同一天阅读至少两篇文章的人，结果按照 id 升序排序。

| article_id | author_id | viewer_id | view_date  |
|  :-------: | :-----------:|:-----------:|:-----------:|
| 1          | 3         | 5         | 2019-08-01 |
| 3          | 4         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |


```sql
select distinct viewer_id as id from views
group by view_date,viewer_id #分组顺序无影响
having count(distinct article_id)>=2
order by id;
```




