---
title: Pandas(1)--读取创建
date: 2020-09-07
tags: ["Pandas","数据清洗"]
categories: ["Pandas"]
author: "Katia"
---

> * 1. 数据读取保存
> * 2. 数据创建

<!--more-->

## 数据读取  

```python
# 相对路径
path = './text.csv'
# 绝对路径
path = r'C:\Users\...\text.csv' (windows)

df = pd.read_csv(path)
df = pd.read_excel(path) 

# 内容有中文 encoding='utf-8'  
# 分隔符设置sep='\t'
# 使用默认列名 header=None
# 指定列名 names=[]
# 跳行读取，不读取1,3,4 行 skiprows = [0,2,3] 
# 默认第一列为索引，第一行为行索引
# 指定行索引 index_col = 
# 只读取前5列 nrows = 
df = pd.read_csv(path,encoding='utf-8',sep='\t',names=['a','b','c'],
	nrows =5,)

# sql读取
import pymysql
conn = pymysql.connect(
	host='',
	user='',
	password='',
	database='',
	charset='')
mysql_page = pd.read_sql('select * from salary',con=conn)


# 默认读取前5行
df.head()
df.head(10)
# 返回行列数
df.shape()
# 返回列名
df.columns()
# 返回索引信息
df.index()
# 返回每列数据类型
df.dtypes()
# 返回整体信息
df.info()
# 返回统计值
df.describe()


文件写入
df.to_csv(path)

```

## 创建seires
```python
s1 = pd.Series([1,'a',5,2.7])
print(s1.index)
print(s1.values)

# 指定index
s1 = pd.Series([1,'g',5,2.7],index=['a','b','c','d'])


# 字典创建series
data = {}
s2 = pd.Series(data)
```

## 创建DataFrame
```python
df1 = DataFrame(np.random.randint(0,150,size=(6,4)),
                columns=['zs','ls','ww','zl'],
                index=[['python','python','math','math','En','En'],
                       ['期中','期末','期中','期末','期中','期末']])
```