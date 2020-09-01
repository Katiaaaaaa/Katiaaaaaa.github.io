---
title: MySQL(3)--排名问题
date: 2020-08-31
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---

> * Top N 
> * 分组排名,每组前几名
> * 组内比较(最值问题)


<!--more-->


##### Top N 查找前20%的数据

用户访问次数表 visit，列名包括user_id、user_type、visit_time。  
要求在剔除访问次数前20%的用户后，每类用户的平均访问次数

1）找出访问次数前20%的用户  
2）剔除访问次数前20%的用户  
3）每类用户的平均访问次数  

```sql
#窗口函数给用户访问量排名
Select * from 
(Select *, row_number() over(order by visit_time desc) as rank
From visit)
 where rank <= max(rank)*0.2

#剔除前20%
Select * from a where rank > (Select max(rank) from a)*0.2

#每类用户的平均访问次数 分组聚合
SELECT AVG(visit_time), type
FROM (
	SELECT *
	FROM (
		SELECT *, row_number() OVER (ORDER BY visit_time DESC) AS rank
		FROM visit
	) a
	WHERE rank > (
		SELECT MAX(rank)
		FROM a
	) * 0.2
) b
GROUP BY type;
```


##### 每组前三名的薪水

找出每个部门获得前三高工资的所有员工

employee:  

| Id | Name  | Salary | DepartmentId |
|  :-------: | :-----------:| :-------:  | :-------:  | 
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |

department:

| Id | Name     |
|  :-------: | :-----------:|
| 1  | IT       |
| 2  | Sales    |


窗口函数解法  
  
先对Employee表进行部门分组工资排名  
再关联Department表查询部门名称  
再使用WHERE筛选出排名小于等于3的数据 

```sql
SELECT B.Name AS Department, A.Name AS Employee, A.Salary
FROM (
	SELECT DENSE_RANK() OVER (PARTITION BY DepartmentId 
		ORDER BY Salary DESC) AS ranking, DepartmentId, Name, Salary
	FROM Employee
) A
	JOIN Department B ON A.DepartmentId = B.id
WHERE A.ranking <= 3;
```


