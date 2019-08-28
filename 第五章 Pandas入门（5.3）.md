## 5.3描述性统计的概述与计算
Pandas对象装配了一个常用数学、统计学方法的集合。与Numpy相比，它内建了处理缺失值的功能。以一个小型DataFrame为例：


```python
import pandas as pd
import numpy as np
obj=pd.DataFrame([[1,2],[3,np.nan],[5,6]],
                index=list('abc'),
                columns=['one','two'])
obj
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
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>a</td>
      <td>1</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>b</td>
      <td>3</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>c</td>
      <td>5</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
obj.sum()  # 列上加和
```




    one    9.0
    two    8.0
    dtype: float64




```python
obj.sum(axis=1)  #行上加和
```




    a     3.0
    b     3.0
    c    11.0
    dtype: float64



通过上述加和我们发现，缺失值被自动排除了，没有对操作造成影响。有些时候我们需要考虑到缺失值，可以使用skipna实现不排除缺失值：


```python
obj.sum(skipna=False)  # 默认情况下skipna是True
```




    one    9.0
    two    NaN
    dtype: float64



还有许多方法，例如 idmin\inmax统计的是最值的索引，cumsum可得到数组累积和，...

需要特别介绍的是describe方法，它可以计算各列的汇总统计集合，而且对于数值型数据和非数值型数据汇总统计结果不同：


```python
obj.describe()  #count表示非NA值的个数
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
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>3.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>3.0</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <td>std</td>
      <td>2.0</td>
      <td>2.828427</td>
    </tr>
    <tr>
      <td>min</td>
      <td>1.0</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>2.0</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>3.0</td>
      <td>4.000000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>4.0</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <td>max</td>
      <td>5.0</td>
      <td>6.000000</td>
    </tr>
  </tbody>
</table>
</div>



### 5.3.1 相关性和协方差
为获得一些在线的DataFrame型数据，考虑使用pip安装pandas-datareader √

相关性 corr()  ,协方差 cov()

### 5.3.2 唯一值、计数和成员属性
unique函数给出序列中的唯一值

value_counts计算Series包含的值的个数

isin执行向量化的成员属性检查


```python
obj=pd.Series(list('aabbccdef'))
obj.unique()
obj.value_counts()
obj.isin['b','c']
```


```python

```


```python

```
