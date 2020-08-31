---
title: MySQL(4)--窗口函数
date: 2020-08-31
tags: ["数据库","数据分析"]
categories: ["MySQL"]
author: "Katia"
---
* Ranking functions
* Analytic functions
* aggregate function

<!--more-->

窗口函数功能：  
window_function_name(expression) 
    OVER (
        [partition_defintion]
        [order_definition]
        [frame_definition]
    )

## Ranking functions
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

percent_rank(): (rank - 1) / (total_rows - 1)  
ntile()


**Frame**
frame_type {boundaries} 

frame_type包括ROW、RANGE两种  
row frame基于 physical offsets ，即基于当前行的位置的偏移量确定计算窗口大小  
range frame基于 logical offsets ，即基于当前行的value值的偏移量来确定计算窗口大小  

boundaries  
UNBOUNDED PRECEDING ， 分区的第一行  
N PRECEDING ，当前行的前向偏移量  
CURRENT ROW , 当前行
N FOLLOWING ，当前行的后向偏移量  
UNBOUNDED FOLLOWING , 表示分区的最后一行  