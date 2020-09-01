---
title: MySQL(1)--最值处理
date: 2020-08-31
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---

> * order by + limit offset
> * 大于等于 max()
> * group by 分组聚合
> * 最值比较 any, all

<!--more-->


## order by + limit 

**使用：不需要分组的情况下取出最值**
```sql
#成绩倒叙排列，取出第一名
select name, score from scores
order by score desc
limit 0,1;
```

**拓展：函数定义**

获取 Employee 表中第 n 高的薪水（Salary）

| <center>Id</center> | <center>Salary </center>|
| :-----: | :-----:  | 
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

```sql
# 函数定义n=n-1, := 为变量赋值
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    SET N := N-1;
  RETURN (SELECT salary FROM employee
      GROUP BY salary
      ORDER BY salary DESC
      LIMIT N, 1);
END
```


## max()函数

**使用：group by + max()**
```sql
#班级分组聚合，取出最值
select class, max(score) from scores
group by class;
```


**拓展：取出第二名成绩**
使用：max()返回最大值作为虚拟表
```sql
select max(distinct score) from scores
where score < (
	select max(distinct score) from scores);
```
注意：分组聚合返回两列值(分组列和聚合列)



**拓展: 子查询对应关系 表连接取最值**

Employee 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id  

| Id | Name  | Salary | DepartmentId |
| :-----: | :-----:  | :-----:  |:-----:  |
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |


Department 表包含公司所有部门的信息。  


| Id | Name     |
| :-----: | :-----:  | 
| 1  | IT       |
| 2  | Sales    |

找出每个部门工资最高的员工

思路：  
对employee表分组聚合取出每个部门的最高员工工资  
内连接两表 并用子查询限定 部门和最高员工工资的关系

```sql
SELECT
	Department.NAME AS Department,
	Employee.NAME AS Employee,
	Salary 
FROM
	Employee,Department 
WHERE
	Employee.DepartmentId = Department.Id 
	AND ( Employee.DepartmentId, Salary )   

IN (SELECT DepartmentId, max( Salary ) 
        FROM Employee 
        GROUP BY DepartmentId )
```





**拓展：分组聚合作为虚拟表 再次分组聚合**

编写一个 SQL 查询，以查询从今天起最多 90 天内，每个日期该日期首次登录的用户数。
假设今天是 2019-06-30.
Traffic 表：

| <center>user_id</center> | <center>activity</center> | <center>activity_date </center>|
| :-----: | :-----:  | :----:  |
| 1       | login    | 2019-05-01    |
| 1       | homepage | 2019-05-01    |
| 1       | logout   | 2019-05-01    |
| 2       | login    | 2019-06-21    |
| 2       | logout   | 2019-06-21    |
| 3       | login    | 2019-01-01    |
| 3       | jobs     | 2019-01-01    |
| 3       | logout   | 2019-01-01    |
| 4       | login    | 2019-06-21    |
| 4       | groups   | 2019-06-21    |
| 4       | logout   | 2019-06-21    |
| 5       | login    | 2019-03-01    |
| 5       | logout   | 2019-03-01    |
| 5       | login    | 2019-06-21    |
| 5       | logout   | 2019-06-21    |

来源：力扣（LeetCode）

```sql 
#用户id分组，取出每位用户最早登陆时间作为虚拟表
#限制时间条件
#虚拟表中以最早登陆时间分组，对用户id计数

select login_date, count(user_id) user_count
from 
(select user_id, min(activity_date) login_date from Traffic
where activity='login'
group by user_id) t
where datediff('2019-06-30',login_date)<=90
group by login_date;
```


## any, all关键字

**any** 标识主查询的条件为满足子查询返回查询结果中任意一条数据记录

匹配模式：  
1. =any 等同于in关键字
2. 大于或大于等于any 比子查询返回的最小数据 要大的数据
3. 小于或小于等于any 比子查询返回的最大数据 要小的数据

**all** 表示主查询的条件为满足子查询返回结果中的所有记录  

匹配模式：  
1. 大于或大于等于all 大于所有
2. 小于或小于等于all 小于所有

```sql
#查询雇员中的姓名和工资，这些雇员的工资不低于职位为manager的工资
select name, sal from employee where sal>
any(select sal from employee where job = 'manager');

#查询雇员的姓名和工资，这些雇员的工资高于职位为manager的工资
select name, sal from employee where sal>
all(select sal from employee where job = 'manager')
```