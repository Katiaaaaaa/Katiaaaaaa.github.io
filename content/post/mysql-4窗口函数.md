---
title: MySQL(4)--常用函数总结
date: 2020-09-01
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---
> * 单行函数
> * 分组函数
> * 窗口函数


<!--more-->

## 单行函数
### 字符串函数
**1. length**   
获取参数值的字节个数
```sql
SELECT LENGTH('katia哈密瓜');  
#utf8中一个汉字3字节一个字母1字节
#看系统编码方式：
SHOW VARIABLES LIKE '%char%';
```

**2. concat**   
拼接字符串
```sql
select concat(last_name,'_',first_name) as name from 
employee;
```

**3. upper/lower**   
大小写转换  
```sql
select concat(upper(last_name),lower(first_name)) as name from employee;
```

**4. substr/substring** 根据索引截取字符 
```sql 
#截取从指定索引处后面的所有字符
select substr('无敌爆炸产出',3) as super;
#截取从指定索引处指定字符长度的字符
SELECT SUBSTR('无敌爆炸产出',1,2); 
#案例：名字中首字符大学，其他字符小写
SELECT CONCAT(UPPER(SUBSTR(last_name,1,1)),LOWER(SUBSTR(last_name,2)))
FROM employees;
```

**5. instr** 返回字符串第一次出现的索引，找不到返回9
```sql
select instr('无敌爆炸产出','爆');
```

**6. trim** 去掉字符前后的符号，没有规定符号的话默认去掉空格
```sql
select trim(' 无敌 ');
select trim('a' from 'aaaa无aa敌aaa');
```

**7. lpad rpad** 用指定的字符实现左填充指定长调度，超过的话从右边截断
```sql
select lpad('pikaqiu',11,'biu');
# biubpikaqiu
select lpad('pikaqiu',2,'biu');
# pi
```  

rpad从右实现，同理

**8. replace** 替换  
```sql
select replace('司凤是男主吧','男','女');
```


## 数学函数

### **1. round**   
四舍五入
```sql
select round(2.13); #默认保留0位小数
select round(2.13,1); #保留一位小数
```

### **2. ceil, floor**   
向上取整 返回整数>=该参数最小整数
```sql
select ceil(2.054); #3 
select ceil(-2.054); #-2 
```
floor 向下取整 返回整数<=该参数最小整数

### **3. truncate**   
截断  
```sql
select truncate(3.1415926,1); 
#保留一位小数
```

### **4. mod**   
求余数
```sql
select mod(10,4);  
```



## 日期函数  

### **1. 返回时间**
```sql
select now();  
select curdate(); 只返回日期
select curtime(); 只返回时间
```

### **2. 字符日期转换**
```sql
date_format() 将日期转换成字符
str_to_date() 将字符转换成日期  
```

### **3.获取指定的部分：年、月、日等** 
```sql 
select Year(now());
select Year(first_login) from players;
```
### **4.monthname:以英文的形式返回月**
```sql
select monthname(first_login) from players;
```
### **5.获取时间间隔**  
datediff(date1,date2) date2-date1  
tiemstamp()


## 流程控制函数

### **1. if函数**
```sql
select if('10>5','大','小');
```
### **2. ifnull函数**  
ifnull(value1, value2)   
如果value1不为空返回value1,否则返回value2
```sql
#如果没有数学第二高的成绩，返回空值
select ifnull(
(select max(distinct score) from scores
where score<(select max(score) from scores where class='math')),null) 
as 'TheSecondHighest_math_score';
```

### **3. case函数**

* 等值判断  
case 要判断的字段或表达式  
when 常量1 then 要显示的值或语句;  
when 常量2 then 要显示的值或语句;  
.......  
else 常量n then 要显示的值或语句;  
```sql
#案例：查询员工的公司，要求 部门号=30，显示的工资1.1，
#部门号=40，显示的工资1.2，部门号=50，显示的工资*1.3...
SELECT salary AS 原始工资,department_id,
CASE department_id
WHEN 30 THEN salary*1.1
WHEN 40 THEN salary*1.2
WHEN 50 THEN salary*1.3
ELSE salary
END AS new_salary
FROM employees;
```
  
* 类似多重if判断，用于判断区间  
case  
when 条件1 then 要显示的值或语句  
when 条件2 then 要显示的值或语句  
when 条件3 then 要显示的值或语句  
......  
else 要显示的值或语句  
end;

```sql
#案例：查询员工工资情况，工资>20000,A,15000,B,10000,C,否则D
SELECT salary,(这有个逗号)
CASE
WHEN salary>20000 THEN 'A'
WHEN salary>15000 THEN 'B'
WHEN salary>10000 THEN 'C'
ELSE 'D'
END AS 工资级别
FROM employees;
```
## 分组函数/统计函数  

### **sum(),avg(),min(),max(),count()**

* sum,avg仅支持数值型，max,min支持字符型（按照首字母由A～Z的顺序排列，越往后，其值越大）
* 与分组函数一同查询的字段是group by后的字段
* 和distinct 搭配   
select sum(distinct salary), sum(salary) from salaries;
* 以上分组函数都忽略null  

## 窗口函数
### **窗口函数功能**  
window_function_name(expression)   
    OVER ([partition_defintion] [order_definition]  
        [frame_definition])  


**Frame子句**  
frame_type {boundaries}      子句用来定义子集的规则，通常用来作为滑动窗口使用


* frame_type包括ROW、RANGE两种  
row frame基于 physical offsets ，即基于当前行的位置的偏移量确定计算窗口大小  
range frame基于 logical offsets ，即基于当前行的value值的偏移量来确定计算窗口大小  

* boundaries  
UNBOUNDED PRECEDING ， 分区的第一行  
N PRECEDING ，当前行的前向偏移量  
CURRENT ROW , 当前行
N FOLLOWING ，当前行的后向偏移量  
UNBOUNDED FOLLOWING , 表示分区的最后一行  

**静态窗口** --无frame功能  
CUME_DIST(), DENSE_RANK(), LAG(),LEAD(),NTILE(), PERCENT_RANK(),RANK(),ROW_NUMBER()  


### **1. 序号函数**
**row_number、rank、dense_rank**  
rank() 排名重叠时，下一名次有间隔  
dense_rank() 排名重叠时，下一名次无间隔  
row_number() 依次排名递增  

empno | ename  | deptno | sal     | num | rank | dense_rank |
|  :-------: | :-----------:| :-------:  | :-------: | :-----------:| :-------:  | :-------: |
|  7839 | KING   |     10 | 5000.00 |   1 |    1 |          1 |
|  7782 | CLARK  |     10 | 2450.00 |   2 |    2 |          2 |
|  7934 | MILLER |     10 | 1300.00 |   3 |    3 |          3 |
|  7788 | SCOTT  |     20 | 3000.00 |   1 |    1 |          1 |
|  7902 | FORD   |     20 | 3000.00 |   2 |    1 |          1 |
|  7566 | JONES  |     20 | 2975.00 |   3 |    3 |          2 |
|  7876 | ADAMS  |     20 | 1100.00 |   4 |    4 |          3 |
|  7369 | SMITH  |     20 |  800.00 |   5 |    5 |          4 |
|  7698 | BLAKE  |     30 | 2850.00 |   1 |    1 |          1 |
|  7499 | ALLEN  |     30 | 1600.00 |   2 |    2 |          2 |
|  7844 | TURNER |     30 | 1500.00 |   3 |    3 |          3 |
|  7521 | WARD   |     30 | 1250.00 |   4 |    4 |          4 |
|  7654 | MARTIN |     30 | 1250.00 |   5 |    4 |          4 |
|  7900 | JAMES  |     30 |  950.00 |   6 |    6 |          5 |  



### **2. 分布函数**  
**percent_rank()/cume_dist()**  
* percent_rank()：(rank - 1) / (rows - 1)  
rank为RANK()函数产生的序号，rows为当前窗口的记录总行数。


* cume_dist()  
用途：分组内小于等于当前rank值的行数/分组内总行数  
应用场景：大于等于当前订单金额的订单比例有多少 

### **3. 前后函数**  
**lead(expre,n)/lag(expre,n)**  
用途：分区中位于当前行前n行（lead）/后n行(lag)的记录值。  
使用场景：查询上一个订单距离当前订单的时间间隔。  

### **4. 头尾函数**  
**first_val(expr)/last_val(expr)**  
用途：得到分区中的第一个/最后一个指定参数的值。  
使用场景：查询截止到当前订单，按照日期排序第一个订单和最后一个订单的订单金额。  

### **5. 其他函数**
nth_value(expr,n)/nfile(n）

* nth_value(expr,n)  
用途：返回窗口中第N个expr的值，expr可以是表达式，也可以是列名。
应用场景：每个用户订单中显示本用户金额排名第二和第三的订单金额。  

* nfile(n)  
用途：将分区中的有序数据分为n个桶，记录桶号。  
应用场景：将每个用户的订单按照订单金额分成3组。
mysql> SELECT
    -> NTILE(3) OVER w AS nf,
    -> stu_id, lesson_id, score
    -> FROM t_score
    -> WHERE lesson_id IN ('L001','L002')
    -> WINDOW w AS (PARTITION BY lesson_id ORDER BY score)
    -> ;



### **6. 聚合函数作为窗口函数**  
用途：在窗口中每条记录动态应用聚合函数(sum/avg/max/min/count)，可以动态计算在指定的窗口内的各种聚合函数值。  
应用场景：每个用户按照订单id，截止到当前的累计订单金额/平均订单金额/最大订单金额/最小订单金额/订单数是多少？  

```sql
select * from 
(select order_id, user_no, amount,
sum(amount) over w as sum1,
avg(amount) over w as avg1,
max(amount) over w as max1,
count(amount) over w as count1
from order
window w as (partition by user_no order by order_id)) t;
```
参考链接：http://www.cnblogs.com/DataArt/p/9961676.html
