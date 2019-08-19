# 第二章 Python语言基础

## 2.1 语言语义

（1）python使用缩进（Tab或空格）来组织代码，一个冒号代表一个缩进代码块的开始。推荐使用四个空格作为默认缩进，而非tab.

（2）一切皆为对象，即所有存在于python解释器中的事物，都是python对象

（3）注释使用#

（4）变量和参数传递 理解python引用语义中复制数据的实际、方法、原因和机制，对于python处理大数据尤为重要

（5）python属于强类型语言。(示例如下) ，在某些语言，如VisualBasic中，字符串'5'可能会隐式地转为整数，得到结果10；在Javascript中，数字5可能会转换成字符串，生成一个结合字符串'55',而python对象只拥有一个指定类型，隐式转换只在某些特定情况发生（如字符串格式化）


```python
'5'+5
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-7-e84694abbbbf> in <module>
    ----> 1 '5'+5
    

    TypeError: can only concatenate str (not "int") to str


（6）isinstance：用于检查对象的类型，也可接受一个包含类型的元组，并检查对象的类型是否在元组类型中，示例如下：


```python
a=5;b=4.5;c='c'
```


```python
isinstance(a,int)
```




    True




```python
isinstance(c,(int,str))
```




    True



（7）验证对象是否可迭代。在编写接受多种类型输入的函数时，常用到该功能。案例：检查对象是否为一个列表，如果不是就把它转换成列表


```python
def isiterable(obj):
    try:
        iter(obj)
        return True
    except TypeError: #对象类型错误，不可遍历
        return False
```


```python
isiterable('hello world')
```




    True




```python
isiterable(5)
```




    False




```python
x=(1,2,3,4,5)
if not isinstance(x,list) and isiterable(x):
    x=list(x)
x
```




    [1, 2, 3, 4, 5]



(8)二元操作符 a= = b VS  a is b

a = = b,a与b相等则为True

a is b ,a与b是同一个python对象则为True

示例如下：a和c值相等，均为[1,2,3]，因此a= =c，但是由于list()函数总是创建一个新的python列表，c相当于拷贝了a，因此a和c并非同一对象，a is not c


```python
a=[1,2,3]
b=a
c=list(a)
```


```python
a==c
```




    True




```python
a is c
```




    False



（9）Python中的可变对象：列表、字典、Numpy数组

Python中的不可变对象：字符串、元组

注意：对象可修改不代表你应该这么做，修改通常会有副作用。因此，建议使用不可变性，尽管不可变对象中也包含可变对象。

## 2.2 标量类型

标准Python标量类型有：None；str；bytes；float；bool；int

2.2.1 字符串补充知识

反斜杠号\是一种转义字符，用于指明特殊符号。如果需要在字符串中写反斜杠号，则需要将其进行转义。例子如下：


```python
s='12\3'
print(s)
```

    12
    


```python
s='12\\3'  #其中一个\的作用是转义字符
print(s)
```

    12\3
    

如果一个字符串中不含特殊符号，但含有大量反斜杠，这时可以在字符串前面加个前缀符号r，表明这些字符是原生字符。r=raw,原生的.


```python
s='this\has\no\special\characters.'
print(s)
```

    this\has
    o\special\characters.
    


```python
s=r'this\has\no\special\characters.'
print(s)
```

    this\has\no\special\characters.
    

2.2.2 字节与Unicode

encode方法可以将Unicode字符串转换为UTF-8字节，decode则相反


```python
val='l love MLA'
val_utf_8=val.encode('utf-8')
val_utf_8
```




    b'l love MLA'




```python
val_utf_8.decode('utf-8')
```




    'l love MLA'



2.2.3 None

None是Python的null值类型，当函数没有显式返回值时就会隐式的返回none.

None还可作为函数的默认参数值


```python
def add_and_maybe_multiply(a,b,c=None):
    result=a+b
    if c is not None:
        result=result*c
    return result
```

从技术角度来说，None不仅是一个关键字，它还是NoneType类型的唯一实例


```python
type(None)
```




    NoneType



2.2.4 时间和日期

Python中内建的datetime模块，提供了datetime,date,time三种类型，使用方法如下：


```python
from datetime import datetime,date,time
dt=datetime(1995,11,26,12,30,15)   #1995年11月26日12点30分15秒
print(dt.date())                   #对于一个datetime实例，可以用 .date()方法获取它的data对象
print(dt.time())                   #同上
print(dt.day)            
print(dt.second)
```

    1995-11-26
    12:30:15
    26
    15
    

两个不同的datetine,会产生一个datatime.timedelta类型的对象（也就是时间间隔）


```python
dt2=datetime(2019,8,19,15,57,50)
delta=dt2-dt
delta
```




    datetime.timedelta(days=8667, seconds=12455)



strftime方法可以将datetime转化为字符串，反之strptime方法可以将字符串转化为datetime对象

%m 两位的月份[0,12]；%d 两位的天数[0,31]；%Y 四位年份； %H 24小时制[00,23]；%M 两位分数值[00,59]


```python
dt.strftime('%m/%d/%Y %H:%M')  
```




    '11/26/1995 12:30'




```python
datetime.strptime('20190819','%Y%m%d')
```




    datetime.datetime(2019, 8, 19, 0, 0)



## 2.3控制流
if是最广为人知的控制流类型 if、elif、else

for循环

while循环

pass 用于在代码段中不执行任何操作，或作为还未实现的代码占位符。之所以需要它是因为Python使用了缩进来分隔代码块

range 函数 range(起始，末尾，步进)返回一个迭代器，生成一个包含起始但不包含末尾的等差整数序列

三元表达式 可压缩代码量但会牺牲可读性，如下：

```python
value=express1 if condition else express2
## 等价于 ##
if condition:
    value=express1
else:
    value=express2
```


```python

```
