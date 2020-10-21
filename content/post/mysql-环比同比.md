---
title: MySQL实战--同比环比计算
date: 2020-10-19
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---

> * sub_date 函数
> * 环比 同比

<!--more-->

## date_sub(date,interval,type)
从日期减去指定的时间间隔

type:
week
hour
day
month
quarter
year
minute
```sql
select date_sub('2019-10-11',interval 2 day);

select city,
count(case when datetime='20190924' then orderId else null end) / 
count(case when datetime=date_sub('20190924',interval 1 week) then orderId else null end)  as '比例' 
from job_interview group by city;


select distinct city,
count(case when datetime='20190924' then orderId else null end) over (partition by city) / 
count(case when datetime=date_sub('20190924',interval 1 week) then orderId else null end) over (partition by city) as '比例' 
from job_interview;
```
