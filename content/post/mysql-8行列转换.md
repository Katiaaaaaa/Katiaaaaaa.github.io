---
title: MySQL(8)--交换数据
date: 2020-09-02
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---

> * 行内容互换
> * 行列内容转换

<!--more-->


## 座位号互换

有一张 seat 座位表，平时用来储存学生名字和与他们相对应的座位 id。其中纵列的 id 是连续递增的。 小美想改变相邻俩学生的座位。  

seat:

|    id   | student |
|  :-------: | :-----------:| 
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |



在原表中插入一列叫做‘奇偶数’，对应表示“座位号”的值是“奇数”还是“偶数”。  
  
1）如果原来座位号是奇数的学生，换座位后，这名学生的座位号变为“座位号+1”。  
2）如果原来座位号是偶数的学生，换座位后，这名学生的座位号变为“座位号-1”。  

sql求余函数：mod(n,m)    返回n除以m的余数。如果n除以2的余数是0，说明n是偶数，否则是奇数


```sql
SELECT CASE 
		WHEN mod(id, 2) != 0
		AND id != counts THEN id + 1
		WHEN mod(id, 2) != 0
		AND id = counts THEN id
		ELSE id - 1
	END AS id, student
FROM seat, (
		SELECT COUNT(*) AS counts
		FROM seat
	) b
ORDER BY id;
```

### 行列互换

Department:

| id   | revenue | month |
|  :-------: | :-----------:| :-----------:| 
| 1    | 8000    | Jan   |
| 2    | 9000    | Jan   |
| 3    | 10000   | Feb   |
| 1    | 7000    | Feb   |
| 1    | 6000    | Mar   |

来源：力扣（LeetCode）

编写一个 SQL 查询来重新格式化表，使得新的表中有一个部门 id 列和一些对应 每个月 的收入（revenue）列

| id   | Jan_Revenue | Feb_Revenue | Mar_Revenue | ... | Dec_Revenue |
|  :-------: | :-----------:| :-----------:|  :-------: | :-----------:| :-----------:| 
| 1    | 8000        | 7000        | 6000        | ... |
| 2    | 9000        | null        | null        | ... | null        |
| 3    | null        | 10000       | null        | ... | null        |


**1. 输出行列互换的表结构** 
Select id, Jan_Revenue, Feb_Revenue... Dec_Revenue from department;  
可以用case语句进行条件判断来替换。id和月份匹配，则为对应值，不匹配则为0。

**2. 去掉0值，简化表格的行数**  
使用分组汇总来实现。按“id”分组，然后用汇总函数（max）取出每组非零的值(也就是这个案例中的id某月对应的数值)。  

```sql
SELECT id, SUM(CASE 
		WHEN month = 'Jan' THEN revenue
	END) AS Jan_Revenue, SUM(CASE 
		WHEN month = 'Feb' THEN revenue
	END) AS Feb_Revenue
	, SUM(CASE 
		WHEN month = 'Mar' THEN revenue
	END) AS Mar_Revenue, SUM(CASE 
		WHEN month = 'Apr' THEN revenue
	END) AS Apr_Revenue
	, SUM(CASE 
		WHEN month = 'May' THEN revenue
	END) AS May_Revenue, SUM(CASE 
		WHEN month = 'Jun' THEN revenue
	END) AS Jun_Revenue
	, SUM(CASE 
		WHEN month = 'Jul' THEN revenue
	END) AS Jul_Revenue, SUM(CASE 
		WHEN month = 'Aug' THEN revenue
	END) AS Aug_Revenue
	, SUM(CASE 
		WHEN month = 'Sep' THEN revenue
	END) AS Sep_Revenue, SUM(CASE 
		WHEN month = 'Oct' THEN revenue
	END) AS Oct_Revenue
	, SUM(CASE 
		WHEN month = 'Nov' THEN revenue
	END) AS Nov_Revenue, SUM(CASE 
		WHEN month = 'Dec' THEN revenue
	END) AS Dec_Revenue
FROM department
GROUP BY id
ORDER BY id;
```

来源：力扣（LeetCode）