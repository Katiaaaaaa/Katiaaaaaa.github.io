---
title: MySQL题库(7)
date: 2020-10-20
tags: ["数据库","数据分析","MySQL-union","MySQL-all"]
categories: ["MySQL"]
author: "Katia"
---

> * union
> * 最值 >all()

<!--more-->

编写一个SQL查询，报告所有雇员最多的项目。

报告所有雇员最多的项目

| project_id  | employee_id |
|  :-------: |:-------: |
| 1           | 1           |
| 1           | 2           |
| 1           | 3           |
| 2           | 1           |
| 2           | 4           |

| employee_id | name   | experience_years |
|  :-------: |:-------: |:-------: |
| 1           | Khaled | 3                |
| 2           | Ali    | 2                |
| 3           | John   | 3                |
| 4           | Doe    | 2                |

```sql
SELECT project_id
FROM project
GROUP BY project_id
HAVING COUNT(DISTINCT employee_id) >= ALL (
	SELECT COUNT(*)
	FROM project
	GROUP BY project_id
);

SELECT project_id
FROM project
GROUP BY project_id
HAVING COUNT(DISTINCT employee_id) = (
	SELECT COUNT(DISTINCT employee_id) AS ctn
	FROM project
	GROUP BY project_id
	ORDER BY ctn DESC
	LIMIT 1
)
```

每一个项目中经验最丰富的雇员是谁
1. 内连接两表,project_id分组,max(experience_years)
```sql
得出每个项目中 最长的年限
select project_id, max(experience_years) as ctn from 
employee e inner join project p 
on e.employee_id = p.employee_id
group by project;

2. 由年限匹配员工编码
project_id,和employee, years 关系在两个表中
所以要用到:
--三表链接
select a.project_id, e.employee_id from
employee e inner join project p 
on e.employee_id = p.employee_id

inner join 

(select project_id, max(experience_years) as ctn from 
employee e inner join project p 
on e.employee_id = p.employee_id
group by project) a 

on a.project_id = p.project_id
and a.ctn = e.experience_years

order by project_id, employee_id;

--子查询两个字段匹配

SELECT
	p.project_id,
	p.employee_id
FROM
	Project AS p
	INNER JOIN Employee AS e
		ON p.employee_id = e.employee_id
WHERE (p.project_id, e.experience_years) IN (
	SELECT
		p.project_id,
		MAX(e.experience_years)
	FROM
		Project AS p
		INNER JOIN Employee AS e
			ON p.employee_id = e.employee_id
	GROUP BY p.project_id
);

```