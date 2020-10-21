---
title: MySQL(9)--建表，增删改
date: 2020-09-02
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---



> * 数据类型
> * 建表
> * 查表
> * 改表
> * 删表

<!--more-->


## 数据类型

### 1. 整数类型  
tinyint 1个字节   
smallint 2个字节    
mediumint 3个字节     
int integer 4个字节 (默认类型)    
bigint 8个字节 (时间戳)  
	 
### 2. 浮点类型/位类型  
float   
double (超过10位，选double)
  
### 3. 时间和日期
date  年和日  
datetime  年月日时分秒  
timestamp 时间戳  
time   
year   

### 4. 字符串类型  
* char系列字符串  
char(M) 0-225 之间的整数  char(4) (长度为4)  
varchar(M) 0-65535 之间的整数  
(需要经常发生变化，选择varchar)
  
* text系列字符串 - 存储大量的字符  
tinytext 字节： 0-225  值得长度为+2 个字节   
text 字节： 0-65 535  值得长度为+2 个字节   
mediumtext 字节： 0-167 772 150  值得长度为+3 个字节   
longtext 字节： 0-4 294 967 295  值得长度为+4 个字节   

* char varchar text 的区别  
都可以存储字符串类型的数据  
char varchar 可以指定最大的字符长度 text 不可以;   
char  自动用空格填充  varchar 不进行空格自动填充;  
text 存储可长度的非空 unicode 数据;  
一个英文一个字节，一个中文四个字节;  


## 建表

### 1. 建表
```sql
#查看所有数据库
SHOW databases;
#进入数据库
USE database;
#查看所有表
SHOW tables;

#建表
CREATE TABLE employee (
	name varchar(4),
	age int,
	birthday datetime
);
```

### 2. 限制列内容 not null
```sql
CREATE TABLE katia (
	name varchar(4) NOT NULL,
	age int,
	birthday datetime
);
```

### 3. default 默认值设置
```sql
CREATE TABLE katiaq (
	name varchar(4) NOT NULL COMMENT '名字',
	age int DEFAULT 18,
	birthday datetime
);
```

### 4. 设置主键
```sql
#建表设置单一主键
CREATE TABLE katiaa (
	id int PRIMARY KEY COMMENT '用户id',
	name varchar(4) NOT NULL COMMENT '名字',
	age int DEFAULT 18,
	birthday datetime
);

#建表设置联合主键
CREATE TABLE katiaaFavorite (
	katiaa_id int COMMENT '用户id',
	article_id int COMMENT '文章id',
	add_time datetime COMMENT '收藏时间',
	PRIMARY KEY (katiaa_id, article_id)
);

#建表后ALTER增加主键
CREATE TABLE katiaaafavorite (
	katiaaa_id int COMMENT '用户id',
	article_id int COMMENT '文章id',
	add_time datetime COMMENT '收藏时间'
);

ALTER TABLE katiaaafavorite
	ADD PRIMARY KEY (katiaaa_id, article_id);

#ALTER删除主键
ALTER TABLE katiaaafavorite
	DROP PRIMARY KEY;
```

### 5. 设置唯一值

```sql
#建表语句设置唯一值
CREATE TABLE katiaa (
	id int UNIQUE COMMENT '用户id',
	name varchar(4) NOT NULL COMMENT '名字',
	age int DEFAULT 18,
	birthday datetime
);

#建表后ALTER语句增加唯一值
CREATE TABLE katiaa (
	katiaa_id int COMMENT '用户id',
	article_id int COMMENT '文章id',
	add_time datetime COMMENT '收藏时间',
	UNIQUE (katiaa_id, article_id)
);

ALTER TABLE katiaa
	ADD UNIQUE KEY (katiaa_id);
```


### 6. 自增长
```sql
#建表设置自增长
CREATE TABLE katia (
	id int PRIMARY KEY AUTO_INCREMENT COMMENT '用户id',
	name varchar(4) NOT NULL COMMENT '名字',
	age int DEFAULT 18,
	birthday datetime
);

#建表后更改自增长
ALTER TABLE katia
	CHANGE COLUMN id id int AUTO_INCREMENT;

#删除自增长
ALTER TABLE katia
	MODIFY COLUMN id int;
```
## 7. 改表
```sql 
-- 删除列
ALTER TABLE vendors DROP COLUMN vend_phone;

-- 添加列：
ALTER TABLE vendors ADD vend_phone CHAR(20);

-- 删除表及结构
DROP TABLE customers;
-- 执行语句将永久删除该表

-- 表重命名
RENAME TABLE  customer TO customers2; 
 alter table film rename to filmm;

-- 修改行内容
UPDATE customers
SET cust_email = 'elmer@fudd.com' WHERE cust_id = 10005;

-- delete 删除一行
DELETE FROM customers WHERE cust_id = 10006; 
-- 若没有WHERE语句，则删除表中每个客户
```