---
title: MySQL--数据导入
date: 2020-10-18
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---


> * python实现数据导入
> * sql空白列设置数字递增
> * sql指定位置新增列

<!--more-->

## python导入数据
```python
from sqlalchemy import create_engine
import pandas as pd 

engine = create_engine('mysql+pymysql://root:8866@localhost:3306/my_test?charset=utf8mb4')
info.to_sql('bf',  con=engine, index=False, if_exists="append")

'''
if_exists 参数
默认 if_exists=fail 自动创建新表，如果数据库已经有该表，会导入失败
if_exists = append 在数据库创建表之后，追加数据
if_exists = replace 如果数据库表有同名表，自动创建新表替换该表

index 参数
index=False 不建立索引
index = True 自动建立索引
'''
```

## datagrip 导入数据
* 需要将excel转化成csv,并且编码为utf-8

## 主键设置删除
```sql
alter table bf add primary key(id);
```
## 指定位置新增列
```sql
alter table bf id int first;
alter table bf id int before user_id;

# 删除行
drop from table bf 
where user_id=10;
# 删除列
alter table bf drop clumns `index`;
```
## 自动填入空白列递增数字
```sql
# 设置变量
select @no :=0;
update bf set id=(@no :=@no+1);

```