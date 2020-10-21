---
title: Numpy
date: 2020-09-22
tags: ["数据分析"]
categories: [" Numpy","Python"]
author: "Katia"
---

<!--more-->

## 创建数组

```python 
# 创建一维数组
# 1. 直接传入列表
import numpy as np
list1 = [1,2,3,4]
oneArray = np.array(list1)
print(type(oneArray))
print(oneArray)
# <class 'numpy.ndarray'>
# [1 2 3 4]

# 2. 传入range生成序列
t2 = np.array(range(10))
print(t2)
# [0 1 2 3 4 5 6 7 8 9]

# 3. 使用numpy的np.arange()生成数组
t3 = np.arange(0,10,2)
print(t3)
# [0 2 4 6 8]
t4 = np.ones(10)
t5 = np.zeros(10)
t6 = np.ones((10,10))
t7 = np.zeros((10,10))


# 创建二维数组
list2 = [[1,2],[3,4],[5,6]]
twoArray = np.array(list2)
print(twoArray)

```
## 常用属性

```python 
list2 = [[1,2],[3,4],[5,6]]
twoArray = np.array(list2)
# 获取数组维度
print(twoArray.ndim)
# 形状
print(twoArray.shape) #(3,2)行列
# 确定数组个数
print(twoArray.size)  # 6 
```
## 调整形状

```python 
# 1 在原有数据上改变
four = np.array([[1,2,3],[4,5,6]])
print(four)
# 3行2列 shape 
four.shape = (3,2)
# [[1 2]
#  [3 4]
#  [5 6]]

# 2 改变形状返回一个新的数据
arr = four.reshape(3,2)
print(arr) 

# 3 二维数组改为一维数组
four = np.array([[1,2,3],[4,5,6]])
four = four.reshape(6,) #6个数据
# F按照列顺序，默认按照C 按照行
arr = arr.reshape((6,),order='F')

# 4 数组的形状
t = np.arange(24)

# 转换为二维
t1 = t.reshape((4,6))
# 转换成三维
t2 = t.reshape((2,3,4))

```

## 数组转为list
```python 
a = np.array([9,12,88,14,25])
list_a = a.tolist()
```
## numpy的数据类型
```python 
#默认是int64
f = np.array([1,2,3,4,5],dtype = np.int16)
# 返回数组中每个元素的字节单位长度
print(f.itemsize)
# 获取数据类型
print(f.dtype)

# 调整数据类型
f1 = f.astype(np.int64)
print(f1.dtype)

# 随机生成小数
arr = np.array([random.random() for i in range(10)])
np.round(arr,2)
```
## 数组的计算
```python 
# 1. 数组和数的计算
# 由于numpy的广播机制在运算过程中，
# 加减乘除的值被广播到所有的元素上面
t1 = np.arange(24).reshape((6,4))
print(t1+2)
print(t1*2)
print(t1/2)

# 同种形状的数组（对应位置进行计算操作）
t1 = np.arange(24).reshape((6,4))
t2 = np.arange(100,124).reshape((6,4))
print(t1+t2)
print(t1*t2)

# 不同形状的多维数组不能计算 行列分布不同
t1 = np.arange(24).reshape((4,6))
t1 = np.arange(18).reshape((3,6))


# 行数或者列数相同的一维数组和多维数组可以进行计算：
# 行形状相同（会与每一行数组的对应位相操作）
t1 = np.arange(24).reshape((4,6))
t2 = np.arange(0,6)
print(t1-t2)
# 列形状相同（会与每一个相同维度的数组的对应位操作）
t1 = np.arange(24).reshape((4,6))
t2 = np.arange(4).reshape((4,1))
print(t1-t2)

```

## 数组的轴
```python 
# 二维的轴
a = np.array([[1,2,3],[4,5,6]])
print(a)
# 根据0轴进行计算 列
print(np.sum(a,axis=0)) #[5,7,9]
# 根据1轴进行计算 行
print(np.sum(a,axis=1)) #[6,15]
print(np.sum(a)) #计算所有和的值


# 三维的轴
import numpy as np
a = np.arange(27).reshape((3,3,3))
print(a)
b = np.sum(a,axis=0)
c = np.sum(a,axis=1)
c = np.sum(a,axis=2)
```

## 数组的索引和切片
```python 
# 一维数组的操作方法
a = np.arange(10)
# 冒号分隔切片参数 start:stop:step 来进行切片操作
# 从索引2开始到索引7停止 (7不取)，间隔为2
print(a[2:7:2]) 

# 如果只放置一个参数，如[2]，将返回与该索引相对应的单个元素
print(a[2],a)

# 如果为[2:],表示从该索引开始以后的所有项都被提取
print(a[2:])

# 多维数组的操作方法
t1 = np.arange(24).reshape(4,6)

# 取一行，根据下标，第一行为0
print(t1[0])
print(t1[0,:])

# 取连续多行
print(t1[1:3])
print(t1[:3],:)

# 不连续取多行:两个中括号
print(t1[[0,2]])
print(t1[[0,2],:]) # 切片[行，列[切片]] [[行行行],[列列列]]
print(t1[[0,1,1],[0,1,3]]) # 0行0列，1行1列，1行3列
print(t1[:,1])  # 取第二列所有值
print(t1[:,1:])  # 第二列到所有列的值

# 取一列
print(t1[:,1])
# 取连续多列
print(t1[:,1:])
# 取不连续多列
print(t1[:,[0,2,3]])
# 取一个值 三行四列
print(t1[2,3]) 
```
## 数组中的数值修改
```python 

t1 = np.arange(24).reshape(4,6)

# 1. 重新赋值

# 修改某一行的值
t[1,:]=0
# 修改某一列的值
t[:,1]=0
# 连续修改多行
t[:3,:]=0
# 连续修改多列
t[:,1:4]=0
# 修改多行多列 第二行到第四行,第三列到第五列
t[1:4,2:5]=0
# 修改多个位置
t[[0,1],[0,3]]=0

# 根据条件修改
# 需求如果大于10，全部变0
t1[t1<10]=0
print(t1)
# 如果大于2 并且小于6 变0
t1[(t1>2) & (t1<6)]=0
print(t1)
# and &, or |, not ~
t1[(t1>2) or (t1<-6)]=0
t[~(t>5)]=0

# np.where
np.where(condition,x,y) 满足条件输出x,否则y
score = np.array[[80,88],[82,81],[75,81]]
np.where(score>80,True,False)

```

## 数组的添加,删除和去重
```python
# 添加
# 1. append 
# 末尾添加值,输入数组维度必须匹配 arr,value,axis
a = np.array([[1,2,3],[4,5,6]])
# axis无定义,展开一维数组,末尾添加
np.append(a,[7,8,9])
# 沿axis=0,添加至最后一行
np.append(a,[7,8,9],axis=0)
# 沿axis=1,添加到末尾列
np.append(a,[[5,5,5],[7,8,9]],axis=1)

# 2. insert
# axis给定轴,否则展开成一维数组 arr index value axis
a = np.array([[1,2],[3,4],[5,6]])
np.insert(a,3,[11,12])
# 沿0轴广播
np.insert(a,1,[11],axis=0)
# [[ 1  2]
#  [11 11]
#  [ 3  4]
#  [ 5  6]]
np.insert(a,3,11,axis=1)

# 删除
arr,obj,axis 
a = np.arange(12).reshape(3,4)
# 删除之前数组被展开
print(np.delete(a,5))
# 删除每一行中的第二列
print(np.delete(a,1,axis=1))

# 数组去重
# numpy.unique 
# arr
# return_index 如果为true,返回新元素在旧列表的位置下标
# return_inverse 如果为true,返回旧元素在新列表的位置下标
# return_counts 如果为true 返回去重数组中的元素在原数组中的出现次数
a = np.array([5,2,5,1,3,3,4,2,2,4])
#去重数组的索引数组
u,indices = np.unique(a,return_index=True)
print(indices)
# #去重数组的下标
u,indices = np.unique(a,return_inverse=True)
print(indices)
# # 返回去重元素的重复数量
u,indices = np.unique(a,return_counts=True)
````
## numpy计算
```python
# 1. 统计函数
np.max() 
np.min()
np.max(score,axis=0)
np.mean()
np.mean(score,axis=0)
# 标准差
np.std(score,axis=0)
# 极值
np.ptp(score,axis=0)
# 协方差
np.var 
# 中位数
np.cov
np.median


# 2. 数据比较
# 第一个参数中的每一个数与第二个参数比较返回大的 广播机制
print(np.maximum([-2,-1,0,1,2],0))  #[0 0 0 1 2]
np.minimum()
# 大小一致，每个元素比较
print(np.maximum([-2,-1,0,1,2],[3,3,3,4,4])) 


# 3. 返回给定axis上的累计和
arr = np.array([[1,2,3],[4,5,6]])
print(arr)
print(arr.cumsum(0))  每行依次累计相加
print(arr.cumsum(1))

# 4. argmin求最小值索引
score = np.array([[80.88],[62,71],[75,81]])
print(np.argmin(score,axis=0))
```
## 数组拼接
```python 
# 1. 维度相同
a = np.array([[1,2],[3,4]])
b = np.array([[5,6],[7,8]]) 
# 四行两列
print(np.concatenate((a,b),axis=0))
print(np.concatenate((a,b),axis=1))

# 2. 根据轴堆叠
# 三维数组
print(np.stack((a,b),axis=0))

# 3. 矩阵拼接

#矩阵垂直拼接
v1 = [[0,1,2,3,4,5],
[6,7,8,9,10,11]]
v2 = [[12,13,14,15,16,17],
[18,19,20,21,22,23]]
# [[ 0  1  2  3  4  5]
#  [ 6  7  8  9 10 11]
#  [12 13 14 15 16 17]
#  [18 19 20 21 22 23]]

#矩阵水平拼接
print(np.hstack((v1,v2)))

```

## 数组分割
```python

# ary 被分割的数组
# indices_or_sections 整数,用平均数切分;数组,沿轴 左开右闭
# axis 轴 默认0 横向
arr=np.arange(9).reshape(3,3)
print(np.split(arr,3))  
#[array([[0, 1, 2]]), array([[3, 4, 5]]), array([[6, 7, 8]])]

# 水平分割
# floor 向下取整
harr = np.floor(10*np.random.random((2,6)))
print(np.hsplit(harr,3))

# 垂直分割
arr=np.arange(16).reshape(4,4)
print(np.vsplit(arr,2))
```
## nan inf 
```python 
a= np.nan
b = np.inf 

# 判断数组中nan的个数(float数组才能赋值nan)
t = np.arange(24,dtype=float).reshape(4,6)

# 判断非0的个数
print(np.count_nonzero(t))

# 三行四列的数改成nan
t[3,4]=np.nan
nan np.nan!=np.nan 
#结果为True


# 结合判断nan的个数
print(np.count_nonzero(t!=t))

#nan和任何数计算都为nan
print(np.sum(t,axis=0))

# 将nan替换为0
t[np.isnan(t)]=0
```

## 二维数组转置
```python

# 对换数组的维度
a = np.arange(12).reshape(3,4)
print(np.transpose(a))
print(a.T)

# 函数用于交换数组的两个轴
t1 = np.arange(24).reshape(4,6)
re = t1.swapaxes(1,0)
```

## 练习
```python

t = np.arange(24).reshape(4,6).astype('float')
# 将数组中的一部分替换nan
t[1,3:] = np.nan
print(t)
# 遍历每一列，然后判断每一列是否有nan
for i in range(t.shape[1]):
    # 获取当前列数据
    temp_col = t[:,i]
    # 判断当前列的数据中是否含有nan
    nan_num = np.count_nonzero(temp_col!=temp_col)
    if nan_num!=0: # 条件成立说明含有nan
        # 将这一列不为nan的数据拿出来
        temp_col_not_nan = temp_col[temp_col==temp_col]
        # 将nan替换成这一列的平均值
        temp_col[np.isnan(temp_col)] = np.mean(temp_col_not_nan)
print(t)
```
