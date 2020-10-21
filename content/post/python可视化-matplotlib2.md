---
title: Matplotlib-绘制不同类型的图像
date: 2020-09-22
tags: ["可视化","数据分析"]
categories: [" Matplotlib","Python"]
author: "Katia"
---


> * 散点图
> * 图像设置

<!--more-->

##  散点图
```python 
# plt.scatter(x, y, marker=None) 函数。
# x、y 是坐标，marker 代表了标记的符号。比如“x”、“>”或者“o”
N = 1000
x = np.random.randn(N)
y = np.random.randn(N)
plt.scatter(x,y,marker='>')
plt.show()


# sns.jointplot(x, y, data=None, kind=‘scatter’) 函数。
# 其中 x、y 是 data 中的下标。
# data 就是我们要传入的数据，一般是 DataFrame 类型。
# kind 这类我们取 scatter，代表散点的意思
# 绘制散点图及变量分布情况
df = pd.DataFrame({'x':x, 'y':y})
sns.jointplot(x='x',y='y',data=df, kind='scatter')
plt.show()
```
![](https://files.catbox.moe/erk29p.png)

## 折线图
```python 
# 折线图
# plt.plot() 函数，需要把数据按照 x 轴的大小进行排序，要不画出来的折线图就无法按照 x 轴递增的顺序展示。
x = [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019]
y = [5, 3, 6, 20, 17, 16, 19, 30, 32, 35]
plt.plot(x,y,color='red',alpha=0.2,linestyle='--',linewidth=3)
#alpha 毛玻璃效果
plt.show()

x = range(1,8)
y = [17,17,18,15,11,11,13]
plt.plot(x,y,marker='o',color='g',alpha = 0.2, markeredgecolor='g',markeredgewidth=5)
plt.show()
#marker实点

# Seaborn 中，使用 sns.lineplot (x, y, data=None) 函数。
# 其中 x、y 是 data 中的下标。data 是要传入的数据，一般是 DataFrame 类型。

df = pd.DataFrame({'x':x,'y':y})
sns.lineplot(x='x',y='y',data=df)
plt.show()
```

## 直方图
```python 
# plt.hist(x, bins=10) 函数
# 其中参数 x 是一维数组
# bins 代表直方图中的箱子数量，默认是 10。
a= np.random.randn(100)
s = pd.Series(a)
plt.hist(s)
plt.show()

# sns.distplot(x, bins=10, kde=True) 函数。
# 其中参数 x 是一维数组，
# bins 代表直方图中的箱子数量，
# kde 代表显示核密度估计，默认是 True，
# 可以把 kde 设置为 False，不进行显示。
# 核密度估计是通过核函数帮我们来估计概率密度的方法。

sns.distplot(s,kde=False)
plt.show()
sns.distplot(s,kde=True)
plt.show()

```
## 条形图
```python 
plt.bar(x, height) 函数，
# 其中参数 x 代表 x 轴的位置序列，
# height 是 y 轴的数值序列，也就是柱子的高度。
x = ['Cat1', 'Cat2', 'Cat3', 'Cat4', 'Cat5']
y = [5, 4, 8, 12, 7]
plt.bar(x,y)
plt.show()

# 使用 sns.barplot(x=None, y=None, data=None) 函数。
# 其中参数 data 为 DataFrame 类型，x、y 是 data 中的变量。
sns.barplot(x,y)
plt.show()
```
## 箱型图
```python 

# 箱型图 盒式图 最大值 (max)、最小值 (min)、中位数 (median) 和上下四分位数 (Q3, Q1)
# plt.boxplot(x, labels=None) 函数，
# 其中参数 x 代表要绘制箱线图的数据，
# labels 是缺省值，可以为箱线图添加标签
data = np.random.normal(size=(10,4))
labels =['A','B','C','D']
plt.boxplot(data,labels=labels)
plt.show()

# sns.boxplot(x=None, y=None, data=None) 函数。
# 其中参数 data 为 DataFrame 类型，x、y 是 data 中的变量。
df = pd.DataFrame(data,columns=labels)
sns.boxplot(data=df)
plt.show()
```



## 饼图
```python 
# plt.pie(x, labels=None) 函数，
# 其中参数 x 代表要绘制饼图的数据，
# labels 是缺省值，可以为饼图添加标签。
nums = [25, 37, 33, 37, 6]
labels = ['High-school','Bachelor','Master','Ph.d', 'Others']
plt.pie(x = nums,labels=labels)
plt.show()
```
![](https://files.catbox.moe/f7fsyt.png)
## 热力图
```python 
# Seaborn 中的 sns.heatmap(data) 函数，
# 其中 data 代表需要绘制的热力图数据。

flights = sns.load_dataset('flights')
data = flights.pivot('year','month','passengers')

sns.heatmap(data)
plt.show()
```
![](https://files.catbox.moe/f13wux.png)

## 蜘蛛图
```python 
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from matplotlib.font_manager import FontProperties
# 数据准备
labels=np.array([u"推进","KDA",u"生存",u"团战",u"发育",u"输出"])
stats=[83, 61, 95, 67, 76, 88]
# 画图数据准备，角度、状态值
angles=np.linspace(0, 2*np.pi, len(labels), endpoint=False)
stats=np.concatenate((stats,[stats[0]]))
angles=np.concatenate((angles,[angles[0]]))
# 用Matplotlib画蜘蛛图
fig = plt.figure()
ax = fig.add_subplot(111, polar=True)
ax.plot(angles, stats, 'o-', linewidth=2)
ax.fill(angles, stats, alpha=0.25)
# 设置中文字体
font = FontProperties(fname=r"C:\Windows\Fonts\simhei.ttf", size=14)
ax.set_thetagrids(angles * 180/np.pi, labels, FontProperties=font)
plt.show()
```
![](https://files.catbox.moe/hp4n9q.png)

## 二元变量分布
```python 
# 两个变量之间的关系
# sns.jointplot(x, y, data=None, kind) 函数。
# 其中用 kind 表示不同的视图类型：“kind=‘scatter’”代表散点图，
# “kind=‘kde’”代表核密度图，
# “kind=‘hex’ ”代表 Hexbin 图，它代表的是直方图的二维模拟。
tips = sns.load_dataset('tips')
sns.jointplot(x='total_bill',y ='tip',data=tips, kind='scatter')
sns.jointplot(x='total_bill',y='tip',data=tips, kind='kde')
sns.jointplot(x='total_bill',y='tip',data=tips,kind='hex')
plt.show()
```
![](https://files.catbox.moe/dddciw.png)
![](https://files.catbox.moe/f7bhrw.png)
![](https://files.catbox.moe/s4mf3n.png)
## 成对关系
```python 
# sns.pairplot() 函数
# 同时展示出 DataFrame 中每对变量的关系，
# 另外在对角线上，你能看到每个变量自身作为单变量的分布情况
iris= sns.load_dataset('iris')
sns.pairplot(iris)
plt.show()
```
![](https://files.catbox.moe/tznll9.png)


