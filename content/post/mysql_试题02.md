---
title: MySQL题库(2)
date: 2020-09-27
tags: ["数据库","数据分析","MySQL-左连接","MySQL-子查询","MySQL-聚合函数"]
categories: ["MySQL"]
author: "Katia"
---

> * 左连接
> * 自连接
> * COUNT() null合计为0

<!--more-->

**leetcode 1364. 顾客的可信联系人数量** 

SQL架构  
顾客表：Customers


| Column Name   | Type    |
|  :-------: | :-----------:|
| customer_id   | int     |
| customer_name | varchar |
| email         | varchar |

customer_id 是这张表的主键。  
此表的每一行包含了某在线商店顾客的姓名和电子邮件。
 

联系方式表：Contacts


| Column Name   | Type    |
|  :-------: | :-----------:|
| user_id       | id      |
| contact_name  | varchar |
| contact_email | varchar |

(user_id, contact_email) 是这张表的主键。  
此表的每一行表示编号为 user_id 的顾客的某位联系人的姓名和电子邮件。  
此表包含每位顾客的联系人信息，但顾客的联系人不一定存在于顾客表中。  
 

发票表：Invoices


| Column Name  | Type    |
|  :-------: | :-----------:|
| invoice_id   | int     |
| price        | int     |
| user_id      | int     |

invoice_id 是这张表的主键。  
此表的每一行分别表示编号为 user_id 的顾客拥有有一张编号为 invoice_id、价格为 price 的发票。  
 

**为每张发票 invoice_id 编写一个SQL查询以查找以下内容：**

customer_name：与发票相关的顾客名称。  
price：发票的价格。  
contacts_cnt：该顾客的联系人数量。  
trusted_contacts_cnt：可信联系人的数量：既是该顾客的联系人又是商店顾客的联系人数量（即：可信联系人的电子邮件存在于客户表中）。  
将查询的结果按照 invoice_id 排序。  

**查询结果的格式如下例所示：**

Customers table:

| customer_id | customer_name | email              |
|  :-------: | :-----------:|:-----------:|
| 1           | Alice         | alice@leetcode.com |
| 2           | Bob           | bob@leetcode.com   |
| 13          | John          | john@leetcode.com  |
| 6           | Alex          | alex@leetcode.com  |

Contacts table:

| user_id     | contact_name | contact_email      |
|  :-------: | :-----------:|:-----------:|
| 1           | Bob          | bob@leetcode.com   |
| 1           | John         | john@leetcode.com  |
| 1           | Jal          | jal@leetcode.com   |
| 2           | Omar         | omar@leetcode.com  |
| 2           | Meir         | meir@leetcode.com  |
| 6           | Alice        | alice@leetcode.com |

Invoices table:

| invoice_id | price | user_id |
|  :-------: | :-----------:|:-----------:|
| 77         | 100   | 1       |
| 88         | 200   | 1       |
| 99         | 300   | 2       |
| 66         | 400   | 2       |
| 55         | 500   | 13      |
| 44         | 60    | 6       |

Result table:

| invoice_id | customer_name | price | contacts_cnt | trusted_contacts_cnt |
|  :-------: | :-----------:|:-----------:|:-----------:|:-----------:|
| 44         | Alex          | 60    | 1            | 1                    |
| 55         | John          | 500   | 0            | 0                    |
| 66         | Bob           | 400   | 2            | 0                    |
| 77         | Alice         | 100   | 3            | 2                    |
| 88         | Alice         | 200   | 3            | 2                    |
| 99         | Bob           | 300   | 2            | 0                    |

Alice 有三位联系人，其中两位(Bob 和 John)是可信联系人。  
Bob 有两位联系人, 他们中的任何一位都不是可信联系人。  
Alex 只有一位联系人(Alice)，并是一位可信联系人。  
John 没有任何联系人。

**解题：左连接，链接数量依次递减**
```sql
SELECT invoices.invoice_id, customers.customer_name, invoices.price, 
	COUNT(contacts.user_id) AS contacts_cnt,
	COUNT(c2.email) AS trusted_contacts_cnt
FROM Invoices invoices
	INNER JOIN Customers customers ON invoices.user_id = customers.customer_id
	LEFT JOIN Contacts contacts ON customers.customer_id = contacts.user_id
	LEFT JOIN Customers c2 ON contacts.contact_email = c2.email
GROUP BY invoices.invoice_id;
```


步骤一：  
发 票 - 顾客 : inner join 寻找发 票对应的顾客信息
顾客 - 联系人 : left join 寻找顾客对应的联系人总数，因为需要统计 0 的情况，所以使用 left 
```sql
SELECT invoice_id, customers.customer_name, price, 
	customers.customer_id, contacts.user_id
FROM Invoices invoices
	INNER JOIN Customers customers ON invoices.user_id = customers.customer_id
	LEFT JOIN Contacts contacts ON customers.customer_id = contacts.user_id
ORDER BY invoice_id;
```


| invoice_id | customer_name | price | customer_id | user_id |
|  :-------: | :-----------:|:-----------:|:-----------:|:-----------:|
|         44 | Alex          |    60 |           6 |       6 |
|         55 | John          |   500 |          13 |    NULL |
|         66 | Bob           |   400 |           2 |       2 |
|         66 | Bob           |   400 |           2 |       2 |
|         77 | Alice         |   100 |           1 |       1 |
|         77 | Alice         |   100 |           1 |       1 |
|         77 | Alice         |   100 |           1 |       1 |
|         88 | Alice         |   200 |           1 |       1 |
|         88 | Alice         |   200 |           1 |       1 |
|         88 | Alice         |   200 |           1 |       1 |
|         99 | Bob           |   300 |           2 |       2 |
|         99 | Bob           |   300 |           2 |       2 |


步骤二：
左连接 联系人 - 顾客 : left join 寻找联系人是否在顾客表中以及总数
```sql
SELECT invoice_id, customers.customer_name, price, 
	customers.customer_id, contacts.user_id, c2.email
FROM Invoices invoices
	INNER JOIN Customers customers ON invoices.user_id = customers.customer_id
	LEFT JOIN Contacts contacts ON customers.customer_id = contacts.user_id
	LEFT JOIN Customers c2 ON contacts.contact_email = c2.email
ORDER BY invoice_id;
```

| invoice_id | customer_name | price | customer_id | user_id | email              |
|  :-------: | :-----------:|:-----------:|:-----------:|:-----------:|:-----------:|
|         44 | Alex          |    60 |           6 |       6 | alice@leetcode.com |
|         55 | John          |   500 |          13 |    NULL | NULL               |
|         66 | Bob           |   400 |           2 |       2 | NULL               |
|         66 | Bob           |   400 |           2 |       2 | NULL               |
|         77 | Alice         |   100 |           1 |       1 | bob@leetcode.com   |
|         77 | Alice         |   100 |           1 |       1 | john@leetcode.com  |
|         77 | Alice         |   100 |           1 |       1 | NULL               |
|         88 | Alice         |   200 |           1 |       1 | john@leetcode.com  |
|         88 | Alice         |   200 |           1 |       1 | NULL               |
|         88 | Alice         |   200 |           1 |       1 | bob@leetcode.com   |
|         99 | Bob           |   300 |           2 |       2 | NULL               |
|         99 | Bob           |   300 |           2 |       2 | NULL               |


