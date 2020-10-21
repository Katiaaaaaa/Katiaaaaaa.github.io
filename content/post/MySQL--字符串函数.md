---
title: MySQL--字符串函数
date: 2020-10-18
tags: ["数据库","数据分析","sql函数"]
categories: ["MySQL"]
author: "Katia"
---


> * 字符串函数

<!--more-->
## 截取字符串

#### substring 截取字符串 

```sql
select substring(string,position)
select substring(string from position)
select substring(string,position,length)

-- string参数是要提取子字符串的字符串
-- position参数是一个整数，用于指定子串的起始字符，position可以是正或负整数。

select substring('my sql',3);
select substring('my sql' from 3);
-- -sql 

select substring('my sql',1,3);
-- -my (有空格)

select substr('my sql',-1);
-- l
```


#### substring_index 按关键字截取字符串 
```sql
substring_index(str,delim,count)
-- 说明：substring_index（被截取字段，关键字，关键字出现的次数） 
select substring_index("blog.jb.net",".",2) as abstract from my_content_t 
-- blog.jb 
-- 如果关键字出现的次数是负数 如-2 则是从后倒数，到字符串结束
```

#### right, left 从两侧截取字符串
```sql
-- left(str,len), right 从最左或最右截取几个字符
select left('wbas',2);
-- wb

```


## 返回字符串长度 length, char_length

#### length 返回字符串存储长度
```sql
select length('text'),length('您好');
-- 4,6
```
#### CHAR_LENGTH(str)：返回字符串中的字符个数
```sql
select char_length('text'),char_length('你好');
-- 4,2
```

#### INSTR(str, substr)：从源字符串str中返回子串substr第一次出现的位置
```sql
select instr('foobarbar','bar');
-- 4
```

## 更改字符串
#### replace 替换文字(大小写敏感) upper,lower
```sql
select replace(str,from_str,to_str);
update tb set f1 = replace(f1,'abc','cdd');
-- 把tb表中的f1字段的abc替换成cdd


-- upper(column/str), lower 把字符串转换成大写或小写
select upper('wbs');
-- WBS
```

#### LPAD(str, len, padstr)：在源字符串的左边填充给定的字符padstr到指定的长度len，返回填充后的字符串
```sql
select lpad('hi',5,'??');
-- ???hi            
```

#### RPAD(str, len, padstr)：在源字符串的右边填充给定的字符padstr到指定的长度len，返回填充后的字符串
```sql
select rpad('hi',6,'??');
-- hi????        
```

#### TRIM([{BOTH | LEADING | TRAILING} [remstr] FROM] str), TRIM([remstr FROM] str)：
```sql
-- 从源字符串str中去掉两端、前缀或后缀字符remstr并返回；
-- 　　如果不指定remstr，则去掉str两端的空格；
-- 　　不指定BOTH、LEADING、TRAILING ，则默认为 BOTH。
select trim('  bar  '); 
-- bar

select trim(leading 'x' from 'xxxbarxxx');
-- barxxx

select trim(both 'x' from 'xxxbarxxx');
-- bar

select trim(trailing 'xyz' from 'barxxyz');
-- barx

```

#### LTRIM(str)，RTRIM(str)：去掉字符串的左边或右边的空格(左对齐、右对齐)
```sql 

SELECT  ltrim('   barbar   ') rs1, rtrim('   barbar   ') rs2;
-- barbar   
#   barbar
```

#### REPEAT(str, count)：将字符串str重复count次后返回
```sql 
select repeat('MySQL',3);
-- MySQLMySQLMySQL 
```
#### REVERSE(str)：将字符串str反转后返回
```sql 
select reverse('abcdef');
--  fedcba 
```

#### FORMAT(X,D[,locale])：以格式‘#,###,###.##’格式化数字X
```sql
-- 　　D指定小数位数
-- 　　locale指定国家语言(默认的locale为en_US)
SELECT format(12332.123456, 4),format(12332.2,0);
-- 12,332.1235  12,332   
```


#### SPACE(N)：返回由N个空格构成的字符串
```sql
 select space(3);
 #    
```
#### STRCMP(expr1,expr2)：如果两个字符串是一样的则返回0；如果第一个小于第二个则返回-1；否则返回1
```sql
select strcmp('text','text'),strcmp('text', 'text2');
-- 0,-1

```

## 合并字符串
#### concat, concat_ws 合并字符串, 管道符号“|”
```sql
-- concat(column/str,column/str2...) 将多个字符串首尾相连后返回
-- 如果有任何参数为null，则函数返回null
-- 如果参数是数字，则自动转换为字符串
select concat(100,'%');
-- 100%


-- concat_ws(separator,str1,str2)
-- 如果有任何参数为null，则函数不返回null，而是直接忽略它
select concat_ws('-','aa','vv');
--  aa-vv


select s_no || s_name || s_age
from student;
-- 进行上式连接查询之后，会将查询结果集在一列中显示(字符串连接)，列名是‘列名1 || 列名2 || 列名3’
 -- s_no || s_name || s_age 
 -- 1001张三23  

```







