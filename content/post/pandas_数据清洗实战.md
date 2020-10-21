---
title: pandas 数据清洗
date: 2020-10-21
tags: ["数据清洗","数据分析"]
categories: ["Pandas"]
author: "Katia"
---
> * 日期函数
> * 文本函数
> * 逻辑函数
> * 统计函数
> * 查找引用函数

<!--more-->

## 数据结构
#### Series 是个定长的字典序列
* Series 有两个基本属性:index 和 values
* index 默认是 0,1,2,……递增的整数序列，当然我们也可以自己来指定索引，比如 index=[‘a’, ‘b’, ‘c’, ‘d’]。
```python 
import pandas as pd 
from pandas import Series, DataFrame
x1 = Series([1,2,3,4])
x2 = Series(data=[1,2,3,4], index=['a', 'b', 'c', 'd'])
print(x1)
# 0    1
# 1    2
# 2    3
# 3    4
print(x2)
# a    1
# b    2
# c    3
# d    4

# 字典的方式创建Series
d = {'a':1, 'b':2, 'c':3, 'd':4}x3 = Series(d)
print x3
```

#### DataFrame 类型数据结构类似数据库表。
* Dataframe 包括了行索引和列索引，我们可以将 DataFrame 看成是由相同索引的 Series 组成的字典类型。

```python 
import pandas as pd
from pandas import Series, DataFrame
df
df1= DataFrame(data)
df2 = DataFrame(data, index=['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'], columns=['English', 'Math', 'Chinese'])
print df1
print df2


#             English  Math  Chinese
# ZhangFei         65    30       66
# GuanYu           85    98       95
# ZhaoYun          92    96       93
# HuangZhong       88    77       90
# DianWei          90    90       80
```

## 数据导入和输出
* Pandas 允许直接从 xlsx，csv 等文件中导入数据，也可以输出到 xlsx, csv 等文件
* 需要安装 xlrd, openpyxl
```python 
import pandas as pd
from pandas import Series, DataFrame
score = DataFrame(pd.read_excel('data.xlsx'))
score.to_excel('data1.xlsx')
print score

pd.read_csv()
pd.to_csv()

```

## 数据清洗

### 1. 删除 DataFrame 中的不必要的列或行
```python 

data = {'Chinese': [66, 95, 93, 90,80],'English': [65, 85, 92, 88, 90],'Math': [30, 98, 96, 77, 90]}
df2 = DataFrame(data, index=['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'], columns=['English', 'Math', 'Chinese'])
# 删除语文列
df2 = df2.drop(columns=['Chinese'])
# 删除张飞行
df2 = df2.drop(index=['ZhangFei'])

```
### 2. 重命名列名 columns，让列表名更容易识别
```python 
df2.rename(columns={'Chinese':'Yuwen','English':'Yingyu'},inplace=True)
```
### 3. 去重复的值

```python 
df = df.drop_duplicates()
```

### 4. 格式问题

##### 更改数据格式
```python 
df2['Chinese'].astype('str')
df2['Chinese'].astype(np.int64)
```
##### 数据间的空格

* 有时候我们先把格式转成了 str 类型，是为了方便对数据进行操作，这时想要删除数据间的空格，我们就可以使用 strip 函数：
```python 
# 删除两边的空格
df2['Chinese'] = df2['Chinese'].map(str.strip)
# 删除左边空格
df2['Chinese'] = df2['Chinese'].map(str.lstrip)
# 删除右边空格
df2['Chinese'] = df2['Chinese'].map(str.rstrip)

```

* 如果数据里有某个特殊的符号，我们想要删除怎么办？同样可以使用 strip 函数，比如 Chinese 字段里有美元符号，我们想把这个删掉，可以这么写：

```python 
df2['Chinese'] = df2['Chinese'].str.strip('$')
```

##### 大小写转换
```python 
# 大写
df2.columns = df2.columns.str.upper()
# 小写
df2.columns = df2.columns.str.lower()
# 首字母大写
df2.columns = df2.columns.str.title()
```

##### 查找空值
```python 
# 哪一列存在空值 
df.isnull().any()
```

## apply函数进行数据清洗

* 对 name 列的数值都进行大写转化可以用：
```python 
# 哪一列存在空值 
df['name'] = df['name'].apply(str.upper)
```
* 也可以定义个函数，在 apply 中进行使用。
比如定义 double_df 函数是将原来的数值 * 2 进行返回。然后对 df1 中的“语文”列的数值进行 * 2 处理
```python 

def double_df(x):
	return 2*x
df1['语文'] = df1['语文'].apply(double_df)

# 直接赋值新增列
df1['语文'] = df1['语文']*2
df1['总和'] = df1['语文']+df1['数学']+df1['英语']
```

比如对于 DataFrame，我们新增两列，其中’new1’列是“语文”和“英语”成绩之和的 m 倍，'new2’列是“语文”和“英语”成绩之和的 n 倍
```python 

def plus(df,n,m):
    df['new1'] = (df['语文']+df['英语']) * m
    df['new2'] = (df['语文']+df['英语']) * n
    return df
df1 = df1.apply(plus,axis=1,args=(2,3,))
```
其中 axis=1 代表按照列为轴进行操作，axis=0 代表按照行为轴进行操作，args 是传递的两个参数，即 n=2, m=3，在 plus 函数中使用到了 n 和 m，从而生成新的 df。


## 数据统计

```python 
df1 = DataFrame({'name':['ZhangFei','Guanyu','a','v','c'], 'data':range(5)})
df1.describe()
#            data
# count  5.000000
# mean   2.000000
# std    1.581139
# min    0.000000
# 25%    1.000000
# 50%    2.000000
# 75%    3.000000
# max    4.000000

```

## 数据表合并
```python 

df1 = DataFrame({'name':['ZhangFei', 'GuanYu', 'a', 'b', 'c'], 'data1':range(5)})
df2 = DataFrame({'name':['ZhangFei', 'GuanYu', 'A', 'B', 'C'], 'data2':range(5)})

```

##### 1. 基于指定列进行连接
```python 
df3 = pd.merge(df1,df2,on='name')
#        name  data1  data2
# 0  ZhangFei      0      0
# 1    GuanYu      1      1
```

#### 2. inner 内连接
inner 内链接是 merge 合并的默认情况，inner 内连接其实也就是键的交集，在这里 df1, df2 相同的键是 name，所以是基于 name 字段做的连接：
```python 

df3 = pd.merge(df1,df2,how='inner')
#        name  data1  data2
# 0  ZhangFei      0      0
# 1    GuanYu      1      1
```

#### 3. left 左连接
左连接是以第一个 DataFrame 为主进行的连接，第二个 DataFrame 作为补充。
```python 

df3 = pd.merge(df1,df2,how='left')
#        name  data1  data2
# 0  ZhangFei      0    0.0
# 1    GuanYu      1    1.0
# 2         a      2    NaN
# 3         b      3    NaN
# 4         c      4    NaN

```

#### 4. right 右连接
右连接是以第二个 DataFrame 为主进行的连接，第一个 DataFrame 作为补充。
```python 
df3 = pd.merge(df1,df2,how='right')
#        name  data1  data2
# 0  ZhangFei    0.0      0
# 1    GuanYu    1.0      1
# 2         A    NaN      2
# 3         B    NaN      3
# 4         C    NaN      4

```


#### 5. outer 外连接
外连接相当于求两个 DataFrame 的并集。
```python 
df3 = pd.merge(df1,df2,how='outer')
#        name  data1  data2
# 0  ZhangFei    0.0    0.0
# 1    GuanYu    1.0    1.0
# 2         a    2.0    NaN
# 3         b    3.0    NaN
# 4         c    4.0    NaN
# 5         A    NaN    2.0
# 6         B    NaN    3.0
# 7         C    NaN    4.0
```


## 用 SQL 方式打开 Pandas

* pandasql 中的主要函数是 sqldf，它接收两个参数：一个 SQL 查询语句，还有一组环境变量 globals() 或 locals()。这样我们就可以在 Python 里，直接用 SQL 语句中对 DataFrame 进行操作

```python 
import pandas as pd
from pandas import DataFrame
from pandasql import sqldf, load_meat, load_births
df1 = DataFrame({'name':['ZhangFei', 'GuanYu', 'a', 'b', 'c'], 'data1':range(5)})
pysqldf = lambda sql: sqldf(sql, globals())
sql = "select * from df1 where name ='ZhangFei'"
print pysqldf(sql)

# lambda 函数
lambda argument_list: expression
pysqldf = lambda sql: sqldf(sql, globals())

```