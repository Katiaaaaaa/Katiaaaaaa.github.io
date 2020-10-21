---
title: Pandas(2)--查询数据
date: 2020-09-07
tags: ["Pandas","数据清洗"]
categories: ["Pandas"]
author: "Katia"
---

> * 1. iloc 行列数字位置
> * 2. loc  行列标签值 既能查询又能覆盖
> * 3. df[]查询  
> * 4. df.at df.iat 单元格查询  

<!--more-->


## 查询方式  
##### 行（列）选取（单维度选取）：df[]。  
这种情况一次只能选取行或者列，即一次选取中，只能为行或者列设置筛选条件（只能为一个维度设置筛选条件）。  

##### 区域选取（多维选取）：df.loc[]，df.iloc[]，df.ix[]。  
这种方式可以同时为多个维度设置筛选条件。

##### 单元格选取（点选取）：df.at[]，df.iat[]。  
准确定位一个单元格。

从dataframe查询
只查询一列返回series;多列返回df


## 导入数据
```python
import pandas as pd 
# 文件内有中文, utf-8 编码
df = pd.read_csv(r'C:\Users\aholaaaaaa\Desktop\katiaaaa\CDA\python show\pandas\data\BJ_tianqi.csv',encoding = 'utf-8')

# 改变索引列，并在原df上修改inplace
df.set_index('日期',inplace = True)

# 查看数据
print(df.info())
print(df.describe())
print(df.head())
'''
         最高温度  最低温度    天气   风向  风级  空气质量
日期                                      
2019/1/1   1℃  -10℃  晴~多云  西北风  1级     良
2019/1/2   1℃   -9℃    多云  东北风  1级     良
2019/1/3   2℃   -7℃     霾  东北风  1级  中度污染
2019/1/4   2℃   -7℃     晴  西北风  2级     优
2019/1/5   0℃   -8℃    多云  东北风  2级     优
'''

df['最低温度'] = df['最低温度'].str.replace('℃','').astype('int32')
df['最高温度'] = df['最高温度'].str.replace('℃','').astype('int32')
print(df.shape)
print(df.index)
print(df.columns)
print(df.dtypes)
```
## 返回多行多列  

loc方法使用方法：DataFrame.loc[ 行索引名称或条件 , 列索引名称 ],闭区间（含最后一个值）  
iloc方法的使用方法：DataFrame.iloc[ 行索引位置 ,  列索引位置 ],开区间（不含最后一个值）

```python
# 返回一行
print(df.loc['2019/1/2'])
print(df.loc['2019/1/2',:])
print(df.iloc[1])
print(df.iloc[1,:])
'''
最高温度     1℃
最低温度    -9℃
天气       多云
风向      东北风
风级       1级
空气质量      良
'''

# 返回多行
print(df.iloc[['1','2']])
print(df.loc[['2019/1/2','2019/1/3']])
print(df.loc[['2019/1/2','2019/1/3'],:])

# 返回一列
print(df.loc[:,'最高温度'])
print(df.iloc[:,0])

# 返回多列
print(df.loc[:,['最高温度','最低温度']])
print(df.iloc[:,[0,1]])
```

## 单个label值查询(行/列)  
```python
print(df.loc['2019/1/3','最高温度'])
print(df.iloc[2,0])
# df.at[]只能使用标签索引，df.iat[]只能使用整数索引
df.at['2019/1/3','最高温度']
df.iat[1,0]
```
## 使用值列表批量查询  
```python
# 一行多列
print(df.loc['2019/1/3',['最高温度','最低温度']])
print(df.iloc[2,[0,1]])
# 多行一列
print(df.loc[['2019/1/3','2019/1/4'],'最高温度'])
print(df.iloc[[2,3],0])
# 多行多列
print(df.loc[['2019/1/3','2019/1/4'],['最高温度','最低温度']])
print(df.iloc[[2,3],[0,1]])
```

## 数值区间查询  
```python 
# 多行
print(df.loc['2019/1/3':'2019/1/4',:])
print(df.loc['2019/1/3':'2019/1/4'])
print(df.iloc[2:4,:])
print(df.iloc[2:4])
# 多列
print(df.loc[:,'最高温度':'最低温度'])
print(df.iloc[:,0:2])
# 多行一列
print(df.loc['2019/1/1':'2019/1/5','最低温度'])
print(df.iloc[0:4,1])
# 一行多列
print(df.loc['2019/1/1','最高温度':'风向'])
print(df.iloc[0,0:3])
# 多行多列
print(df.loc['2019/1/1':'2019/1/5','最高温度':'风向'])
print(df.iloc[0:4,0:3])
```
## 使用条件表达式查询
```python
# 筛选行
print(df.loc[df['最低温度']<-10,:])
perfect_w = df.loc[(df['最高温度']<=15)&(df['天气']=='晴')&(df['空气质量']=='良'),:]

# 对行和列同时进行筛选
print(df.loc[df['最低温度']<-10,['最低温度','空气质量']])
```
## 调用函数查询  
```python
content = df.loc[lambda df: (df['最高温度']<=30) & (df['最低温度']>=15), :]

def query_my_data(df):
	return (df.index.str.startswith['2019/9'])& (df['空气质量'])=='良'
	
print(df.loc[query_my_data,:])
```
 
## df[] 查询
```python
# 整数索引切片：前闭后开
# 选取第一行
df[0:1]
# 选取前两行
df[0:2]

# 标签索引切片：前闭后闭
# 选取第一行：
df[:'2019/1/1']
# 选取前两行:
df['2019/1/1','2019/1/2']

# 布尔数组
# 所有最高温度大于15的行
print(df[[each > 15 for each in df['最高温度']]])
print(df[df['最高温度']>15])

# 选取所有最高温度大于15的行且空气质量为良的行
df[(df['最高温度']>15) & (df['空气质量']=='良')]

# 选取所有最高温度为15或20的行
df[(df['最高温度']==15) |(df['最高温度']==20)]

# 列选取：
# 列选取方式也有三种：标签索引、标签列表、Callable对象

# a）标签索引：选取单个列
df['最高温度']
# b）标签列表：选取多个列
df[['最高温度','最低温度']]
# c）callable对象
# 选取第一列
df[lambda df:df.columns[0]]
```

