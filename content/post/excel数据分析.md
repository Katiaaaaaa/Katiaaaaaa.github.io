---
title: excel函数
date: 2020-09-01
tags: ["数据清洗","数据分析"]
categories: ["Excel"]
author: "Katia"
---
> * 日期函数
> * 文本函数
> * 逻辑函数
> * 统计函数
> * 查找引用函数

<!--more-->

## 日期函数

#### 1.Date

功能：将提取的数字变为日期格式进行显示。

语法：DATE（year,month,day）

释义：公式中的三个参数分别为年，月，日，对应填入就可以将数字组合成为日期。


#### 2. Day

功能：返回一个月中的第几天的数值，介于1到31之间。

语法：day(serial_number)

释义：Serial_number为要查找的天数日期。
日期有多种输入方式：带引号的文本串（例如"1998/01/30"）、
系列数（例如，如果使用 1900 日期系统则 35825 表示 1998 年 1 月 30 日）或其他公式或函数的结果（例如 DATEVALUE("1998/1/30")）。

#### 3. Edate

功能：用于计算指定日期之前或之后几个月的具体日期。

语法：EDATE(start_date,months)

释义：start_date：表示起始日期的日期。

months：表示start_date 之前或之后的月份数。

#### 4.Eomonth

功能：计算指定日期之前或者之后几个月的日期，返回结果日期的当月最后一天。

语法：EOMONTH(start_date, months)

释义：Start_date 是代表开始日期的一个日期。

Months为 start_date 之前或之后的月数。正数表示未来日期，负数表示过去日期。


#### 5. Hour

功能：用于返回时间值中的小时数，返回的值范围是0～23。

语法：HOUR(serial_number)

释义：serial_number：表示要提取小时数的时间。

#### 6.Minute

功能：返回一个指定时间值中的分钟数。

语法：MINUTE(serial_number)

释义：Serial_number 必需。一个时间值，其中包含要查找的分钟。时间值有多种输入方式：带引号的文本字符串（例如 "6:45 PM"）、十进制数（例如 0.78125 表示 6:45 PM）或其他公式或函数的结果（例如 TIMEVALUE("6:45 PM")）。

#### 7.Month

功能：返回月份值，且返回的值是1到12之间的整数。

语法：MONTH（serial_number)

释义：Serial_number 必须存在，含义：要查找的月份日期。


#### 8.Networkdays

功能：返回开始日期和结束日期之间的所有工作日数，其中，工作日包括周末和专门指定的假期。

语法：NETWORKDAYS(start_date,end_date,holidays)

释义：start_date:表示开始日期。

end_date:表示结束日期。

holidays:在工作日中排除的特定日期。

#### 9. today 
功能：返回系统的当前日期。
语法：today( )
释义：该函数没有参数，只用一对括号即可。
2020/01/01 


#### 10.now 
功能：返回系统的当前日期和时间。
语法：now( )
释义：该函数没有参数，只用一对括号即可。
2020/01/01 12:00:00

#### 11.Second

功能：返回一个时间值中的秒数。

语法：SECOND(serial_number)

释义：serial_number：表示要提取秒数的时间。一分钟只有60秒，函数结果的取值范围是0-59


#### 12.Weekday

功能：返回代表一周中的第几天的数值，是一个1到7之间的整数。

语法：WEEKDAY(serial_number，return_type)

释义：serial_number  是要返回日期数的日期
return_type为确定返回值类型的数字，数字1 则1 至7 代表星期天到星期六，数字2 则1 至7 代表星期一到星期天，数字3则0至6代表星期一到星期天。


#### 13.Weeknum

功能：返回位于一年中的第几周

语法：WEEKNUM（serial_num，return_type）

释义：参数Seria_num 必须。代表要确定它位于一年中的几周的特定日期。

参数Return_type 可选。为一数字，它确定星期计算从哪一天开始，其默认值为1，其有两种系统：

系统1包含本年度1月1日的周为本年度第一周，即为第1周。

系统2包含本年度第一个星期四的周为本年度第一周，即为第一周。本系统基于ISO 8601，即为欧洲星期计数系统。

#### 14.Workday

功能：返回在某日期（起始日期）之前或之后、与该日期相隔指定工作日的某一日期的日期值。

语法：WORKDAY(start_date, days, [holidays])

释义：Start_date必需。一个代表开始日期的日期。
Days 为正值将生成未来日期；为负值生成过去日期。


#### 15.Year

功能：返回日期的年份值，一个1900-9999之间的数字。

语法：YEAR(serial_number)

释义：Serial_number 为一个日期值，其中包含要查找的年份


#### 16.Datedif

功能: DATEDIF函数是Excel隐藏函数，其在帮助和插入公式里面没有。返回两个日期之间的年月日间隔数。

语法: DATEDIF(start_date,end_date,unit)

释义: Start_date 为一个日期，它代表时间段内的第一个日期或起始日期。(起始日期必须在1900年之后)

End_date 为一个日期，它代表时间段内的最后一个日期或结束日期。

Unit 为所需信息的返回类型。

=DATEDIF(A1,TODAY(),"Y")计算年数差
=DATEDIF(A1,TODAY(),"M")计算月数差
=DATEDIF(A1,TODAY(),"D")计算天数差
"MD" 起始日期与结束日期的同月间隔天数。
"YD" 起始日期与结束日期的同年间隔天数。
"YM" 起始日期与结束日期的同年间隔月数。


## 文本函数

文本中提取字符

#### left 

=LEFT(text, [num_chars])
其中text为要取得给定值的文本数据源，
num_chars表示需要从左开始算提取几个字符数，其中每个字符按1计数。

例如“=LEFT(12345678,3)”，表示从字符”12345678“中取前三位字符，运行的结果为123。


#### right
=RIGHT(text,[num_chars])，

text为要取得给定值的文本数据源
num_chars表示需要从右开始算提取几个字符数，其中每个字符按1计数。

如下“=RIGHT(E3,4)”，表示从产品编号“sh3137200”中取后四位编码，运行的结果为“7200”


#### mid
=MID(text, start_num, num_chars)，其中text为要取得给定值的文本数据源， 
start_num表示指定从第几位开始提取，
num_chars表示需要从指定位置开始算提取几个字符数，其中每个字符按1计数。例如：“=MID(E3,3,3)”表示从产品编号”sh3137200“中的第三位开始取三位字符，运行的结果为313。

#### len函数

语法：len(文本)

作用：返回文本中字符串的个数


文本合并
获取文本信息
#### Find函数Find函数指对要查找的文本进行定位，以确定其位置。

Find函数的语法格式：=Find(find_text,within_text,start_num)Find（要查找的文本，文本所在的单元格，从第几个字符开始查找[可选，省略默认为1，从第一个开始查找]）。
764911118@qq.com
=find("1",A1,1) 3 

#### 2.search函数

语法：search(要查找的字符串，文本，[从左起第几个查找])

作用：返回一个字符串在另一个字符串中出现的位置（不区分大小写）

#### 3.replace函数

语法：replace(文本，从左起第几个，替换几个，新的字符)

作用：将一个字符串的部分字符替换成另一个字符串


三、英文大小写函数

#### 1.upper函数

语法：upper(英文文本)

作用：将所有的英文字符转化成大写

#### 2.lower函数

语法：lower(英文文本)

作用：将所有的英文字符转化成小写

#### 3.proper函数

语法：proper(英文文本)

作用：将英文字符转化首字母大写，其余小写


文本清洗

#### 1.trim函数

语法：trim(文本)

作用：除了保留字符之间的单个空格外，移除其余所有空格

#### 2.text函数

语法：text(数字，要转化成的文本格式 )

作用：将数值转成指定格式的文本

2019/1/2
text(A1,"mm月dd日")

##数字函数

#### sum
#### sumif
sumifs
rand
randbetween
round

取绝对值
=abs(数字)

取整
=int(数字)

四舍五入
=round(数字)


##统计函数

counta
countif 
countifs
average
averageif
averageifs
large
small
max
 min
 median
 modd
 rank 

##逻辑函数
if 
iferror

=IFERROR(A2/B2,"")

说明：如果是错误值则显示为空，否则正常显示。
and 
or 
ture
false

##查找引用函数
index
match
lookup
vlookup
VLOOKUP函数语法：
=VLOOKUP(查找值,在哪个区域查找,返回区域第几列,精确或模糊匹配)

第4参数为0时为精确匹配，1时为模糊匹配。
hlookup
row
column
choose
indirect 
offset 
