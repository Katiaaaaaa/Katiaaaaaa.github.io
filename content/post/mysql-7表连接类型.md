---
title: MySQL(7)--多表查询
date: 2020-09-02
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---

> * INNER JOIN 内连接
> * OUTER JOIN 外连接
> * CROSS JOIN 交叉连接
> * UNION 并集查询
> * 自连接

<!--more-->

![](https://files.catbox.moe/43gwfq.png)

## **内连接/等值连接**  
取两表交集部分 根据每个表共有的列的值匹配两个表中的行   
  
* 隐式内连接  
```sql
SELECT *  
FROM A, B  
WHERE A.id = B.id 
``` 
　　  
* 显式内连接  
```sql
SELECT *  
FROM A  
	INNER JOIN B ON A.id = B.id 
``` 

**示例：**  
employee:  

| employee_id | department_id |
|  :-------: | :-----------:| 
|           1 |             1 |
|           2 |             2 |
|           3 |             2 |

salary:  

| id   | employee_id | amount | pay_date   |
|  :--: | :-------:|  :-------: | :-------:| 
|    1 |           1 |   9000 | 2017-03-31 |
|    2 |           2 |   6000 | 2017-03-31 |
|    3 |           3 |  10000 | 2017-03-31 |
|    4 |           1 |   7000 | 2017-02-28 |
|    5 |           2 |   6000 | 2017-02-28 |
|    6 |           3 |   8000 | 2017-02-28 |


```sql
SELECT e.employee_id, e.department_id, s.amount
FROM employee e
	INNER JOIN salary s ON e.employee_id = s.employee_id;
```
内连接表：   

| employee_id | department_id | amount |
|  :-------: | :-----------:|  :-------: | 
|           1 |             1 |   9000 |
|           2 |             2 |   6000 |
|           3 |             2 |  10000 |
|           1 |             1 |   7000 |
|           2 |             2 |   6000 |
|           3 |             2 |   8000 |

## **外连接**  
如果使用左连接，则主表在它左边；如果使用右连接，则主表在它右边 
查询结果以主表为主，从表记录匹配不到，则填充null
 
* 左外连接: LEFT (OUTER) JOIN   
```sql
SELECT e.employee_id, e.department_id, s.amount  
FROM employee e
	LEFT JOIN salary s ON e.employee_id = s.employee_id;

SELECT e.employee_id, e.department_id, s.amount  
FROM employee e
	LEFT JOIN salary s ON e.employee_id = s.employee_id
	where ;

``` 




* 右外连接: RIGHT (OUTER) JOIN  

```sql
SELECT e.employee_id, e.department_id, s.amount  
FROM employee e
	RIGHT JOIN salary s ON e.employee_id = s.employee_id;
```  

* 取坐标


## **交叉连接/笛卡尔积**  
返回的结果为被连接的两个数据表的乘积  

* 隐式交叉连接
```sql  
SELECT * 
FROM  A, B;
```

* 显式交叉连接
```sql  
SELECT *
FROM A
	CROSS JOIN B;
```


## using子句简化表连接
```sql
SELECT *
FROM employee
	JOIN dept ON employee.deptno = dept.deptno;

SELECT *
FROM employee
	JOIN dept USING (deptno);
```


## 三表连接
```sql
SELECT last_name, first_name, dept_name
FROM employees a
	LEFT JOIN dept_emp b ON a.emp_no = b.emp_no
	LEFT JOIN departments c ON b.dept_no = c.dept_no;
```