---
title: Matplotlib-图像的基础设置
date: 2020-09-22
tags: ["可视化","数据分析"]
categories: [" Matplotlib","Python"]
author: "Katia"
---


> * 基础绘图
> * 图像设置

<!--more-->

## 1. 基础绘图
```python
# 1. 导入模块,包（模块集合）
import matplotlib.pyplot as plt
# 在jupyter中执行的时候显示图片
# %matplotlib inline (jupter 提供)
# 传入x,y 通过plot画图
plt.plot([1,0,9],[4,5,6])
# 在执行程序的时候展示图形
plt.show()
```
![](https://files.catbox.moe/0l84th.png)

## 2. 颜色和形状设置
```python
x = range(1,8)
y = [17,18,19,20,21,22,23]
plt.plot(x,y,color="red",alpha = 0.2, linestyle="--",linewidth = 3) #(颜色)
# alpha 毛玻璃效果(0-1) 折线透明度
# linestyle 折线的样式 ： solid实线,dashed短线, dashdot端点相间线, dotted 虚点线
# linewidth 折线的宽度
plt.show()
```
![](https://files.catbox.moe/fhqcw5.png)


## 3. 折点样式
```python
x = range(1,8)
y = [17,17,18,15,11,11,13]
plt.plot(x,y,marker='o',color='g',alpha = 0.2, markeredgecolor='g',markeredgewidth=5)
# marker 实点
plt.show()
```
![](https://files.catbox.moe/b61i9h.png)

## 4. 图片的大小和保存
```python
import random
x = range(2,26,2)
y = [random.randint(15,30) for i in x]
plt.figure(figsize=(20,8),dpi=80)
plt.plot(x,y)

# figsize：指定figure的宽和高，单位为英寸
# dpi参数指定绘图对象的分辨率，即每英寸多少个像素，缺省值为80,
# 1英寸等于2.5cm

# plt.show() #相当于close,内容释放
# 保存（注意：要放在绘制的下面，并且plt.show()会释放figure资源，如果现实图像之前释放则保存图像为空
plt.savefig('./t1.png')
# 图片的格式也可以保存为svg矢量图格式，放大不会有锯齿
plt.savefig('./t1.svg')
```
![](https://files.catbox.moe/6nx31c.png)


## 5. 绘制x轴和y轴的刻度

```python
x = range(2,26,2)
y = [random.randint(15,30) for i in x]
plt.figure(figsize=(20,8),dpi=80)

#  构造x轴刻度标签
# 2:00 x_ticks_label 显示标签，X确定个数和疏密程度
x_ticks_label = ['{}:00'.format(i) for i in x]
# rotation = 45 让字旋转45度
plt.xticks(x,x_ticks_label,rotation = 45)
# 设置y轴的刻度标签
# 构造显示的标签
y_ticks_label = ['{}℃'.format(i) for i in range(min(y),max(y)+1)]
# 绘图
plt.plot(x,y)
plt.show()
````
![](https://files.catbox.moe/frr9sb.png)  

## 6. 设置显示中文
```python
# matplotlib只显示英文，无法显示中文，需要修改 matplotlib 的默认字体
# 通过 matplotlib下的font_manager 可以解决
# 两个小时的每分钟跳动变化
from matplotlib import font_manager
x = range(0,120)
y = [random.randint(10,30) for i in range(120)]

plt.figure(figsize=(20,8), dpi=80) #设置画布的大小和分辨率
plt.plot(x,y)

# 加坐标轴信息
# 查看Linux,Mac下支持的字体
# 	终端执行：fc-list
# 	查看支持的中文： fc-list :lang=zh
# 查看windows下的字体："C:\Windows\Fonts\"

# 可以自己下载字体文件 (xxx.ttf),然后双击安装即可
# my_font = font_manager.FontProperties(fname='/System/Library/Fonts/PingFang.ttc',s)
# plt.ylabel('天气',fontproperties=my_font)

# 构造字体
my_font = font_manager.FontProperties(fname='\Windows\Fonts\simfang.ttf',size=20, weight=100)
# 设置X轴标题
plt.xlabel('时间',rotation=45, fontproperties=my_font)
# rotation 将字体旋转45度
plt.ylabel('次数',fontproperties=my_font)
plt.title('每分钟跳动次数',fontproperties=my_font,color='red')
plt.show()
```
![](https://files.catbox.moe/nwxlzz.png)


## 7. 一图多线
```python
y1 = [1,0,1,1,2,4,3,4,4,5,6,5,4,3,3,1,1,1,1,1]
y2 = [1,0,3,1,2,2,3,4,3,2,1,2,1,1,1,1,1,1,1,1]
x = range(11,31)
# 设置图形
plt.figure(figsize=(20,8),dpi=80)
# 图例显示的文字
plt.plot(x,y1,color='r',label='自己')
plt.plot(x,y2,color='b',label='同事')
# 设置x轴刻度
x_ticks_label = ['{}岁'.format(i) for i in x]
my_font = font_manager.FontProperties(fname='\Windows\Fonts\simfang.ttf',size=18)
plt.xticks(x,x_ticks_label,fontproperties=my_font,rotation=45)
# 绘制网格（网格也可以是设置线的样式）
plt.grid(alpha=0.4)
# 添加图例（注意：只有在这里需要添加prop参数是显示中文，其他的都用fontproperties
# 设置位置loc:upper left, lower left, center left, upper center
# 设置中文显示字体prop
plt.legend(prop=my_font,loc='upper right')
plt.show()
```
![](https://files.catbox.moe/tg2ac9.png)
## 8. 一图多个坐标系子图
```python
# add_subplot 方法----给figure新增子图

x = np.arange(1,100)
# 新建figure对象
fig = plt.figure(figsize=(20,10),dpi=80)
# 新建子图1
# (2,2,1)两行两列 画布上布局
ax1 = fig.add_subplot(3,2,1)
ax1.plot(x,x)
# 新建子图2
ax2 = fig.add_subplot(3,2,2)
ax2.plot(x,x**2)
# grid网格
ax2.grid(color='r',linestyle='--',linewidth=1,alpha=0.3)
# 新建子图3
ax3 = fig.add_subplot(3,2,3)
ax3.plot(x,np.log(x))
plt.show()
```
![](https://files.catbox.moe/e79y14.png)

## 9.  设置坐标轴范围
```python
x = np.arange(-10,11,1)
y = x**2
plt.plot(x,y)
# 可以调x轴的左右两边
# plt.xlim([-5,5])
# 只调一边
# plt.xlim(xmin=-4)
# plt.xlim(xmax=4)
plt.ylim(ymin=0)
plt.xlim(xmin=0)
plt.show()
```
![](https://files.catbox.moe/e2d6a8.png)

## 10. 改变坐标轴的默认显示方式
```python
import matplotlib.pyplot as plt
import numpy as np
y = range(0,14,2)
x = [-3,-2,-1,0,1,2,3]
plt.figure(figsize=(20,8),dpi=80)

# 获得当前图表的图像
ax = plt.gca()
# 设置图型的包围线
ax.spines['right'].set_color('none')  #不显示
ax.spines['top'].set_color('none')  #不显示
ax.spines['bottom'].set_color('b')
ax.spines['left'].set_color('r')

# 设置底边的移动范围，移动到y轴的0位置，
# ‘data’：移动轴的位置到交叉的指定坐标
ax.spines['bottom'].set_position(('data',0))
ax.spines['left'].set_position(('data',1))
plt.plot(x,y)
plt.show()
```
![](https://files.catbox.moe/ckou6j.png)


