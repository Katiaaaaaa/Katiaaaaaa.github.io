---
title: MySQL(2)--日期函数及格式
date: 2020-08-31
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---

> * datediff(),tiemstamp()
> * 日期格式
> * 限制日期条件

<!--more-->

## 时间函数
datediff(date1,date2)=date1-date2

timestampdiff(timetype,date1,date2)=date2-date1  
timetype = 'day','hour','second'...

```sql
#找出第二天销售额多余第一天销售额的日期和销售额，产品id
Select a.id, a.date, a.amount  from sales a cross join sales b
On diffdate(a.date – b.date) = 1
Where a.amount > b.amount；
```


## 日期格式

**DATE_FORMA(date, format)** 日期格式转换成字符串
**str_to_data** 字符串转换成日期格式

```sql
date_format(pay_date,'%Y-%m') as pay_month
```

%S,%s 两位数字形式的秒（ 01, . . ., 59）  
%i 两位数字形式的分  
%H 两位数字形式的小时，24 小时  
%h, %I 两位数字形式的小时，12 小时（01,02, . . ., 12）  
%k 数字形式的小时，24 小时（0,1, . . ., 23）  
%l 数字形式的小时，12 小时（1, 2, . . ., 12）  
%T 24 小时的时间形式（h h : m m : s s）  
%r 12 小时的时间形式（hh:mm:ss AM 或hh:mm:ss PM）  
%p AM 或P M  
%W 一周中每一天的名称（ S u n d a y, Monday, . . ., Saturday）  
%a 一周中每一天名称的缩写（ Sun, Mon, . . ., Sat）  
%d 两位数字表示月中的天数（ 00, 01, . . ., 31）  
%e 数字形式表示月中的天数（ 1, 2， . . ., 31）  
%D 英文后缀表示月中的天数（ 1st, 2nd, 3rd, . . .）  
%w 以数字形式表示周中的天数（ 0 = S u n d a y, 1=Monday, . . ., 6=Saturday）  
%j 以三位数字表示年中的天数（ 001, 002, . . ., 366）  
% U 周（0, 1, 52），其中Sunday 为周中的第一天  
%u 周（0, 1, 52），其中Monday 为周中的第一天  
%M 月名（J a n u a r y, February, . . ., December）  
%b 缩写的月名（ J a n u a r y, February, . . ., December）  
%m 两位数字表示的月份（ 01, 02, . . ., 12）  
%c 数字表示的月份（ 1, 2, . . ., 12）  
%Y 四位数字表示的年份  
%y 两位数字表示的年份  
%% 直接值“%”  

## 时间段筛选

**max(),min() 创建时间段**

报告2019年春季才售出的产品。
即仅在2019-01-01至2019-03-31（含）之间出售的商品。

table product:

| product_id | product_name | unit_price |
|  :-------: | :-----------:| :-------:  | 
| 1          | S8           | 1000       |
| 2          | G4           | 800        |
| 3          | iPhone       | 1400       |

sales table:

| seller_id | product_id | buyer_id | sale_date  | quantity | price |
| :-----: | :-----:  |  :-----:  | :-----: | :-----:  |  :-----:  | 
| 1         | 1          | 1        | 2019-01-21 | 2        | 2000  |
| 1         | 2          | 2        | 2019-02-17 | 1        | 800   |
| 2         | 2          | 3        | 2019-06-02 | 1        | 800   |
| 3         | 3          | 4        | 2019-05-13 | 2        | 2800  |


```sql
#产品分组，having筛选出时间最值
select s.product_id, product_name from product p 
inner join sales s 
on p.product_id = s.product_id
group by s.product_id 
having max(sale_date)<='2019-03-31' and min(sale_date)>='2019-01-01';
```

**between 'date1' and 'date2'**