title: python 中的时间函数
date: 2015-06-13 15:54:29
categories:
- python
tags:
- time

---

###python时间函数###

```python
import time         # 时间戳相关
import datetime     # 日期相关
```

*time中的常用函数*
```python
In [3]: time.
time.accept2dyear  time.asctime       time.ctime         time.gmtime        time.mktime        time.strftime      time.struct_time   time.timezone      time.tzset
time.altzone       time.clock         time.daylight      time.localtime     time.sleep         time.strptime      time.time          time.tzname
```

<!--more-->

---

####time模块####

*时间戳与asctime*
```python
In [1]: time.time()  # 获取当前的时间戳，时间从格林尼治时间1970年1月1号0时计算
Out[1]: 1434189463.564346

In [2]: time.ctime(0)  # 北京是东8区
Out[2]: 'Thu Jan  1 08:00:00 1970'

In [3]: time.asctime()  # 输出asctime格式时间
Out[3]: 'Sat Jun 13 17:54:03 2015'

In [4]: time.ctime(time.time())  # 将当前的时间戳转化为asctime格式
Out[4]: 'Sat Jun 13 17:57:53 2015'
```


*gmtime*
```python
In [1]: gmtime = time.gmtime()  # 获取struct_time格式的时间,可使用时间戳作为输入,不输入则为当前时间

In [2]: gmtime
Out[2]: time.struct_time(tm_year=2015, tm_mon=6, tm_mday=15, tm_hour=2, tm_min=29, tm_sec=53, tm_wday=0, tm_yday=166, tm_isdst=0)  # 包含年月日时分秒，还有星期几和该年的第多少天可能比较有用

# 可直接获取子属性值
In [3]: gmtime.
gmtime.n_fields           gmtime.n_unnamed_fields   gmtime.tm_isdst           gmtime.tm_min             gmtime.tm_sec             gmtime.tm_yday
gmtime.n_sequence_fields  gmtime.tm_hour            gmtime.tm_mday            gmtime.tm_mon             gmtime.tm_wday            gmtime.tm_year
```


*mktime*
```python
# 找了个w3cschool上的例子试了下，mktime和gmtime是相反的操作，要求必须输入9个int值，但生成时间戳只用到了前6个，我这里故意输错了后边的，结果没有影响
In [1]: t = (2009, 2, 17, 17, 3, 38, 2, 49, 0)

In [2]: time.mktime(t)
Out[2]: 1234861418.0

In [3]: time.gmtime(time.mktime(t))
Out[3]: time.struct_time(tm_year=2009, tm_mon=2, tm_mday=17, tm_hour=9, tm_min=3, tm_sec=38, tm_wday=1, tm_yday=48, tm_isdst=0)

# 如果参数不够9个会报错
In [4]: t = (2009, 2, 17, 17, 3, 38)
In [5]: time.mktime(t)
TypeError: argument must be sequence of length 9, not 6
```


*strftime*
```python
# time.strftime(format[, t])
# 将struet_time格式的时间转换为所需要的时间字符串形式，不传入struet_time则显示当前时间的
```
**字符串格式可以参考<http://www.w3cschool.cc/python/att-time-strftime.html>**


