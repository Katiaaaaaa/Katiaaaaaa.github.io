---
title: MySQL实战--备份,替换内容
date: 2020-10-19
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---
> * 备份数据
> * 拆解datetime列
> * replace替换列内容
> * 缺失值处理

<!--more-->


## 复制表数据表结构
```sql
# 复制表结构及数据到新表
create table table_sample select * from table limit 10;
# 只复制表结构到新表
create table table_sample like table;
# 复制旧表的数据到新表(假设两个表结构一样) 
insert into table_sample select * from table;
# 复制旧表的数据到新表(假设两个表结构不一样) 
INSERT INTO 新表(字段1,字段2,.......) SELECT 字段1,字段2,...... FROM 旧表 
```

## 列名time,格式datetime 拆解成date 和time两列

```sql
-- 先创建列
alter table user add hour time;
alter table user add date date;
-- 利用date_format
update user set hour = date_format(time,'%H:%i:%s')
update user set date = date_format(time,'%y-%m-%d')
```


## 替换列内容
```sql
-- 由于 behavior_type 列的四种行为类型分别用 1，2，3，4 表示点击、收藏、加购物车、购买四种行为，为了方便查看数据，将1，2，3，4替换为 ‘pv’、’fav‘，’cart’，‘buy’ 。

-- behavior 之前是tinyint结构，需要更改成varchar 再进行替换文字

alter table user modify behavior_type varchar(20);
update user set behavior_type = replace(behavior_type,1,'pv');
update user set behavior_type = replace(behavior_type,2,'fav');
update user set behavior_type = replace(behavior_type,3,'cart');
update user set behavior_type = replace(behavior_type,4,'buy');
```

## 缺失值处理：
1. 建立联合主键避免缺失
2. 过滤缺失值
```sql
select * from user where user_id is not null and length(user_id)>0;
```