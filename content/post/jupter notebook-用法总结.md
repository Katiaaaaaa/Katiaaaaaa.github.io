---
title: jupyter notebook--使用方法
date: 2020-09-01
tags: ["python","md格式","Jupyter Notebook"]
categories: ["python"]
author: "Katia"
---


> * 改变页面显示字体
> * 页面主题下载
> * 代码自动补全设置
> * code,md 格式

<!--more-->

## 改变页面显示字体
1. 改变浏览器字体
2. pip install -- upgrade jupyterthemes

## 页面主题下载
* cmd 查看主题 jt -l , 更改主题 jt -t oceans16  
* 恢复默认主题 jt -r
* 设置主题参数
-t 设置主题，-f设置代码字体
jt -t oceans16 -f consolamono -nf robotosans -tf robotosans -N -T -cellw 60% -dfs 9 -ofs 9

## 代码自动补全设置
* pip install jupyter_contrib_nbextensions  安装显示目录
* jupyter contrib nbextension install --user --skip-running-check 关闭jupyter notebook
* 打开jupyter notebook, 选择 Nbextensions, 勾选Hinterland

## code,md 格式
* 绿色-编辑模式
* m 进入markdown模式
* h 查看快捷键
* 回车 命令模式-编辑模式
* esc  编辑模式-命令模式
* shift+enter 运行代码块并跳转下一个代码块
* ctrl + enter 只运行当前代码块
* alt + enter 运行代码块并在下方新建一个代码块
* 蓝色-命令模式 m markdown单元格  y代码单元格   shift enter 渲染
* 蓝色-命令模式 b 在下方创建代码块
* 蓝色-命令模式 a 在上方创建代码块
* 蓝色-命令模式  x 剪切 shift+v 粘贴到上面 v粘贴到当前代码块
* 蓝色-命令模式 z恢复 d删除
* 命令模式 l 给多行代码标行数
* c 复制单元格，v粘贴新的单元格
* $数学公式$


## markdown
*  #  一级标题
*  ## 二级标题
* > 引用
* **加粗**
* *斜体*
* [百度](www.baidu.com) 链接
* ![腾讯视频](link/w宽度/1240)  图片



* @[toc] 文章目录

* 嵌入api
<iframe frameborder='0' width='600' height='300'>
src =''
<iframe>
源代码放入 html


