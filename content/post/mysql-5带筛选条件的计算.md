---
title: MySQL(5)--case when 运用计算
date: 2020-09-01
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---


> * case when 在带条件筛选中的运用
<!--more-->

## 输出占比  

统计每个班同学各科成绩平均分大于80分的人数和人数占比  
table student: name, id, class, age, major  
table score: id, class_id, score  

（1）每位同学的平均成绩  
（2）平均分大于80分的人数  
（3）平均分大于80分的人数占比  
（4）输出结果是班级，平均分大于80分的人数，平均分大于80分的人数占比  

-每位同学的平均成绩
```sql
select id,avg(score) as avg_score
from score group by id;
```
-平均分大于80分的人数  
```sql
select sum(case when avg(score)>80 then 1 else 0 end) as num from 
(select id,avg(score) as avg_score
from score group by id) a 
```
-平均分大于80分的人数占比   
```sql
select sum(case when avg(score)>80 then 1 else 0 end)/
count(id) as percentage
from 
(select id,avg(score) as avg_score
from score group by id) a
```
-输出结果是班级、人数、人数占比
```sql
select s.class, num, percentage from student as s
left join a on s.id = a.id
group by class
```
-综合
```sql
select s.class, 
sum(case when avg(score)>80 then 1 else 0 end) as num, 
sum(case when avg(score)>80 then 1 else 0 end)/count(id) as percentage
from student as s left join 
(select id,avg(score) as avg_score
from score group by id) a on s.id = a.id
group by class;
```

**模板总结： 有筛选条件的统计数量问题**  
```sql
select sum(
case when <判断表达式> then 1else 0
end) as 数量 from table x;
```

![](https://files.catbox.moe/dhgyoy.png)


http://music.163.com/song/28287132/?userid=446906676
