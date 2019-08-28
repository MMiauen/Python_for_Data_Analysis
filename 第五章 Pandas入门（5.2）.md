## 5.2 基本功能
本节将指引你了解与Series或DataFrame中数据交互的基础机制
### 5.2.1 重建索引
reindex是Pandas对象的重要方法，该方法可以创建一个符合新索引的新对象。当调用该方法时，数据会按照新的索引重新排序，若某个索引值之前不存在，则会引入缺失值。


```python
import pandas as pd
obj=pd.Series([-3,4,-1,9],index=['a','c','b','d'])
obj
```




    a   -3
    c    4
    b   -1
    d    9
    dtype: int64




```python
obj2=obj.reindex(['a','b','c','d','e'])
obj2
```




    a   -3.0
    b   -1.0
    c    4.0
    d    9.0
    e    NaN
    dtype: float64



对于顺序数据，例如时间数据，若重建索引时需要进行插值或填值，method可选参数中：ffill方法会将值向前填充、bfill方法会将值向后填充。


```python
obj3=pd.Series(['bule','red','yellow'],index=[2,5,7])
obj3
```




    2      bule
    5       red
    7    yellow
    dtype: object




```python
obj3.reindex(range(8),method='ffill')  # ffill方法顺着往下填充
```




    0       NaN
    1       NaN
    2      bule
    3      bule
    4      bule
    5       red
    6       red
    7    yellow
    dtype: object




```python
obj3.reindex(range(8),method='bfill')   # bfill 方法逆着往上填充
```




    0      bule
    1      bule
    2      bule
    3       red
    4       red
    5       red
    6    yellow
    7    yellow
    dtype: object



重建索引：改变行索引、使用columns关键字改变列索引、使用loc同时改变行索引和列索引


```python
import numpy as np
frame=pd.DataFrame(np.arange(9).reshape((3,3)),             #3x3方阵
                  index=['a','c','d'],                      #行索引
                  columns=['xiamen','beijing','shanghai'])  #列索引
frame
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>xiamen</th>
      <th>beijing</th>
      <th>shanghai</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <td>c</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <td>d</td>
      <td>6</td>
      <td>7</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame.reindex(['a','b','c','d'])       # 改变行索引
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>xiamen</th>
      <th>beijing</th>
      <th>shanghai</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>b</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>c</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <td>d</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>8.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame.reindex(columns=['beijing','shanghai','xiamen'])  #改变列索引
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>beijing</th>
      <th>shanghai</th>
      <th>xiamen</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>c</td>
      <td>4</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <td>d</td>
      <td>7</td>
      <td>8</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame.loc[['a','b','c','d'],['beijing','shanghai','xiamen']]   # loc[行索引，列索引] ，这种方式更加简洁
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>beijing</th>
      <th>shanghai</th>
      <th>xiamen</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>b</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>c</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>d</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
</div>



### 5.2.2 轴向上删除条目
本小节将介绍drop方法，该方法会返回一个经删除修改后的新对象,原对象没有变。

默认情况下，调用drop是根据行标签去删除相应的值，若要在列上删除值，需要传递参数 axis=1或axis='columns'


```python
data=pd.DataFrame(np.arange(16).reshape((4,4)),
                 index=['one','two','three','four'],
                 columns=['Apple','Banana','Orange','Pear'])
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Apple</th>
      <th>Banana</th>
      <th>Orange</th>
      <th>Pear</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>two</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>three</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>four</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.drop('two')   # 默认删除行数据，要是想删多列要写成列表形式['two','four']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Apple</th>
      <th>Banana</th>
      <th>Orange</th>
      <th>Pear</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>three</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>four</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.drop('Orange',axis='columns')  #若不指定是列，则会报错：KeyError: "['Orange'] not found in axis"
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Apple</th>
      <th>Banana</th>
      <th>Pear</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>0</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <td>two</td>
      <td>4</td>
      <td>5</td>
      <td>7</td>
    </tr>
    <tr>
      <td>three</td>
      <td>8</td>
      <td>9</td>
      <td>11</td>
    </tr>
    <tr>
      <td>four</td>
      <td>12</td>
      <td>13</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




```python
data                              # 即使经过了上述删除操作，原对象data是未被改变的
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Apple</th>
      <th>Banana</th>
      <th>Orange</th>
      <th>Pear</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>two</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>three</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>four</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>



以上操作均生成新对象，而不会修改原数据data。若想直接在原数据上进行操作，可以这样：obj.drop('索引',inplace=True)，inplace可以清除被删除的数据。

### 5.2.3 索引、选择与过滤
使用索引loc(轴标签）和iloc(整数标签）以Numpy风格的语法，从DataFrame中选出数组的行和列的子集。


```python
data=pd.DataFrame(np.arange(16).reshape((4,4)),
                 index=['one','two','three','four'],
                 columns=['Apple','Banana','Orange','Pear'])
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Apple</th>
      <th>Banana</th>
      <th>Orange</th>
      <th>Pear</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>two</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>three</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>four</td>
      <td>12</td>
      <td>13</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.loc['one',['Apple','Pear']]  #单行多列
```




    Apple    0
    Pear     3
    Name: one, dtype: int32




```python
data.loc['two':'three','Apple':'Orange']  #多行多列
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Apple</th>
      <th>Banana</th>
      <th>Orange</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>two</td>
      <td>4</td>
      <td>5</td>
      <td>6</td>
    </tr>
    <tr>
      <td>three</td>
      <td>8</td>
      <td>9</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.iloc[2,[3,0,1]]
```




    Pear      11
    Apple      8
    Banana     9
    Name: three, dtype: int32



### 5.2.4 整数索引
### 5.2.5 算术和数据对齐
两个不同索引对象之间进行相加，在没有交叠的标签位置上，内部数据对齐会产生缺失值NaN，若将两个行列完全不同的对象相加，结果将全为空。


```python
s1=pd.Series([1,2,3],['a','b','c'])
s2=pd.Series([4,5,6],['b','c','d'])
s1+s2
```




    a    NaN
    b    6.0
    c    8.0
    d    NaN
    dtype: float64



对于不同标签对象之间存在的差异，我们可以将其填充为0，再进行算术操作，以免影响后续操作：


```python
obj1=pd.DataFrame(np.arange(9).reshape((3,3)),
                columns=list('abc') )
obj1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <td>2</td>
      <td>6</td>
      <td>7</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
obj2=pd.DataFrame(np.arange(16).reshape((4,4)),
                 columns=list('abcd'))
obj2.loc[1,'b']=np.nan      #把第二行第'b'列的元素设为空值
obj2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0</td>
      <td>1.0</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4</td>
      <td>NaN</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <td>2</td>
      <td>8</td>
      <td>9.0</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>3</td>
      <td>12</td>
      <td>13.0</td>
      <td>14</td>
      <td>15</td>
    </tr>
  </tbody>
</table>
</div>




```python
obj1+obj2    #未进行填充之前，两对象相加结果是这样滴，有很多空值
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>1</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>11.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2</td>
      <td>14.0</td>
      <td>16.0</td>
      <td>18.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



我们可以对obj1使用add算术方法，再把obj2以及填充值传入：


```python
obj1.add(obj2,fill_value=0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>7.0</td>
      <td>4.0</td>
      <td>11.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>14.0</td>
      <td>16.0</td>
      <td>18.0</td>
      <td>11.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>12.0</td>
      <td>13.0</td>
      <td>14.0</td>
      <td>15.0</td>
    </tr>
  </tbody>
</table>
</div>



类似的算数方法还有很多：add\sub\div\floordiv整除\mul\pow幂次方，每个方法都可以传入填充值fill_value，而且每个方法都有一个r开头的副本，副本方法的参数是翻转的。例如：obj.div(x) >>> obj/x  ;obj.rdiv(x) >>> x/obj

下面我们通过一个例子来认识什么是广播机制:

当我们从arr-arr[0]时，减法在每一行都进行了操作，这就是广播机制。广播机制同样适用于Series和DataFrame中。

如果你要在列上进行广播，在行上匹配，那么被减数要取列，而且axis='index'或axis=0.  示例：obj1.sub(被减列，axis='index')


```python
arr=np.arange(12).reshape((3,4))
print('arr:',arr)
print('arr[0]:',arr[0])
print('arr-arr[0]:',arr-arr[0])
```

    arr: [[ 0  1  2  3]
     [ 4  5  6  7]
     [ 8  9 10 11]]
    arr[0]: [0 1 2 3]
    arr-arr[0]: [[0 0 0 0]
     [4 4 4 4]
     [8 8 8 8]]
    

### 5.2.6 函数应用和映射 
Numpy的通用函数对Pandas对象同样有效，例如np.abs()等，是对数组进行逐元素操作

另一常见操作是将函数应用到一行或一列的一维数组上，DataFrame的apply方法可以实现这个功能：


```python
frame=pd.DataFrame(np.arange(9).reshape((3,3)),
                  index=['one','two','three'],
                  columns=list('abc'))
frame
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <td>two</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <td>three</td>
      <td>6</td>
      <td>7</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
f=lambda x:x.max()-x.min()   # 函数f是计算最大值与最小值间的差值
frame.apply(f)                #计算每列上的最值差值
```




    a    6
    b    6
    c    6
    dtype: int64




```python
frame.apply(f,axis='columns')   #计算每行上的最值差值
```




    one      2
    two      2
    three    2
    dtype: int64



### 5.2.7 排序和排名
根据某些准则对数据集进行排序是一个重要的内建操作。使用sort_index方法，可以根据行或列索引进行字典型排序，，并返回一个新的排序好的对象。


```python
frame=pd.DataFrame(np.arange(9).reshape((3,3)),
                  index=['c','a','b'],
                  columns=[2,3,1])
frame
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2</th>
      <th>3</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>c</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <td>a</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <td>b</td>
      <td>6</td>
      <td>7</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame.sort_index()   # 默认是在行索引上进行排序，且默认为升序排列
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2</th>
      <th>3</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <td>b</td>
      <td>6</td>
      <td>7</td>
      <td>8</td>
    </tr>
    <tr>
      <td>c</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
frame.sort_index(axis='columns',ascending=False)  #列索引上进行排序需要axis=1或axis='columns',还可以设置为降序
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>3</th>
      <th>2</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>c</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <td>a</td>
      <td>4</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <td>b</td>
      <td>7</td>
      <td>6</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



除了可以根据索引进行排序之外，还可以根据序列的值进行排序。方法：sort_values

无论是默认情况下的升序排列，还是自己设置成降序排列，所有缺失值都会被排在Series的尾部

其实sort_values还有一个可选参数by,可以用于选择某列或多列作为排序键


```python
obj=pd.Series([3,-2,np.nan,9,-13,0,np.nan])
obj.sort_values()
```




    4   -13.0
    1    -2.0
    5     0.0
    0     3.0
    3     9.0
    2     NaN
    6     NaN
    dtype: float64



Series和DataFrame中实现排名的方法是rank，默认方法是'average'在组中平均分配排名；还有‘first’方法，按照数值在数组中出现顺序排名；...


```python
obj=pd.Series([3,-1,2,3])
obj.rank()   # 两个元素3本应都是排名第3，但每个排名只能有一个位置，所以它们只能占第3和第4这两个位置，再平均一下，（3+4）/ 2 = 3.5
```




    0    3.5
    1    1.0
    2    2.0
    3    3.5
    dtype: float64




```python
obj.rank(method='first')   # 第一个3比第4个3排名靠前，这是因为，标签0在标签3的前面，先观察到它。这也是'first'方法的本质
```




    0    3.0
    1    1.0
    2    2.0
    3    4.0
    dtype: float64



### 5.2.8 含有重复标签的轴索引
我们遇到以及常用的对象索引都是唯一的，但这并不是强制性的。下面我们来看数组具有重复索引的情况：


```python
obj=pd.Series(range(5),['a','a','b','b','c'])
obj
```




    a    0
    a    1
    b    2
    b    3
    c    4
    dtype: int64




```python
obj.index.is_unique   # 使用is_unique属性可以告诉你它的标签是否唯一
```




    False




```python
obj['a']
```




    a    0
    a    1
    dtype: int64



当标签重复时，检索该标签会出现所有符合条件的值（会返回一个序列），而单个条目返回的是标量值。

DataFrame中同理。


```python

```
