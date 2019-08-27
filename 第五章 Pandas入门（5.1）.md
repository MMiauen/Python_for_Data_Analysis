# 第五章 Pandas入门
与适合处理数值类型数组数据的Numpy相比，Pandas更适应于处理表格型的异质型数据，它所包含的数据结构和数据处理工具是的在Python中进行数据清洗和分析非常快捷。入门Pandas，需要熟悉两个最常用的工具数据结构：Series和 DataFrame，这是非常重要的基础。

pip安装pandas遇到问题：(Caused by SSLError）

解决办法：pip install pandas -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
## 5.1 Pandas数据结构介绍
### 5.1.1 Series
Series是一种一维数组对象，它由索引和值构成。


```python
import pandas as pd

obj=pd.Series([3,5,7,9])
obj   #索引在左值在右
```




    0    3
    1    5
    2    7
    3    9
    dtype: int64




```python
obj.values  #values属性可获取Series对象的值
```




    array([3, 5, 7, 9], dtype=int64)




```python
obj.index #index属性可获取Series的索引
```




    RangeIndex(start=0, stop=4, step=1)



通常，索引序列是可以自己创建的，可以自己使用标签标识每个数据点.选择数据时也可以按照自定义的标签进行索引。


```python
obj1=pd.Series([1,2,3],index=['a','b','c'])
obj1
```




    a    1
    b    2
    c    3
    dtype: int64




```python
obj1['b']
```




    2



如果现在已经有一个字典，可以使用该字典生成一个Series.而且，当你把字典传递给Series构造函数时，Series索引序列将是排序好的字典键。如果你想自己决定索引顺序，可以把字典键按照你想要的顺序传递给构造函数：


```python
dic={'N01':100,'NO2':'95','NO3':90,'NO4':80}
obj=pd.Series(dic)
obj
```




    N01    100
    NO2     95
    NO3     90
    NO4     80
    dtype: object




```python
dic={'N01':100,'NO2':'95','NO3':90,'NO4':80}
my_index=['NO2','N01','NO4','NO5','NO3']         
dic_to_Series=pd.Series(dic,my_index)
dic_to_Series
```




    NO2     95
    N01    100
    NO4     80
    NO5    NaN
    NO3     90
    dtype: object



上述例子中，NO1-NO4的值都被放在对应的位置上，但是由于NO5没有出现在dic的键中，所以它对应的值是NaN（not a number），这是pandas标记缺失值的方式。

Pandas中使用isnull和notnull函数来检查缺失数据：(如何处理缺失值将在第七章深入讨论)


```python
pd.isnull(dic_to_Series)   #检查序列中缺失值部分。这也是个实例方法，可以这样使用：dic_to_Series.isnull()
```




    NO2    False
    N01    False
    NO4    False
    NO5     True
    NO3    False
    dtype: bool




```python
pd.notnull(dic_to_Series)
```




    NO2     True
    N01     True
    NO4     True
    NO5    False
    NO3     True
    dtype: bool



Series对象自身及其索引都有name属性，也就是说可以对Series序列包括其索引进行命名：


```python
dic_to_Series.name='Test'   #为序列命名
dic_to_Series.index.name='grade'  #为序列的索引命名
dic_to_Series
```




    grade
    NO2     95
    N01    100
    NO4     80
    NO5    NaN
    NO3     90
    Name: Test, dtype: object



Series的索引还可以通过按位置赋值的方式进行改变：


```python
dic_to_Series.index=['Bob','Jim','Kitty','Nancy','Amy']
dic_to_Series.index.name="name"
dic_to_Series
```




    name
    Bob       95
    Jim      100
    Kitty     80
    Nancy    NaN
    Amy       90
    Name: Test, dtype: object



### 5.1.2 DataFrame
DataFrame表示矩阵的数据表，它是二维的。DataFrame既有行索引又有列索引，它可以被视为一个共享相同索引的Series的字典。而且，从DataFrame中选取的列是数据的视图而不是拷贝，因此，对Series的修改会直接映射到DataFrame中。如果需要复制，应当显式的使用Series的copy方法。

最常用的构建DataFrame的方法：利用包含等长度列表或Numpy数组的字典来形成DataFrame。如下：


```python
data={'name':['Bob','Jim','Kitty','Nancy','Amy','lily'],
     'year':[1995,1997,1999,2000,2012,1999],
     'color':['red','yellow','green','black','white','red']}
Frame=pd.DataFrame(data)
Frame
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
      <th>name</th>
      <th>year</th>
      <th>color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Bob</td>
      <td>1995</td>
      <td>red</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Jim</td>
      <td>1997</td>
      <td>yellow</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Kitty</td>
      <td>1999</td>
      <td>green</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Nancy</td>
      <td>2000</td>
      <td>black</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Amy</td>
      <td>2012</td>
      <td>white</td>
    </tr>
    <tr>
      <td>5</td>
      <td>lily</td>
      <td>1999</td>
      <td>red</td>
    </tr>
  </tbody>
</table>
</div>



默认情况下，DataFrame的列是按照所传字典的顺序排列的，但是你也可以自己指定列的顺序：


```python
Frame=pd.DataFrame(data,columns=['year','color','name'])
Frame
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
      <th>year</th>
      <th>color</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1995</td>
      <td>red</td>
      <td>Bob</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1997</td>
      <td>yellow</td>
      <td>Jim</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1999</td>
      <td>green</td>
      <td>Kitty</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2000</td>
      <td>black</td>
      <td>Nancy</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2012</td>
      <td>white</td>
      <td>Amy</td>
    </tr>
    <tr>
      <td>5</td>
      <td>1999</td>
      <td>red</td>
      <td>lily</td>
    </tr>
  </tbody>
</table>
</div>



如果你传入了不包含在字典中的列，结果将会出现缺失值：


```python
Frame=pd.DataFrame(data,columns=['year','color','name','country'],index=['one','two','three','four','five','six'])
Frame
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
      <th>year</th>
      <th>color</th>
      <th>name</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>1995</td>
      <td>red</td>
      <td>Bob</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>two</td>
      <td>1997</td>
      <td>yellow</td>
      <td>Jim</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>three</td>
      <td>1999</td>
      <td>green</td>
      <td>Kitty</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2000</td>
      <td>black</td>
      <td>Nancy</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2012</td>
      <td>white</td>
      <td>Amy</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>six</td>
      <td>1999</td>
      <td>red</td>
      <td>lily</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



对于大型DataFrame，head（）函数只会选出对象的前五行：


```python
Frame.head()
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
      <th>year</th>
      <th>color</th>
      <th>name</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>1995</td>
      <td>red</td>
      <td>Bob</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>two</td>
      <td>1997</td>
      <td>yellow</td>
      <td>Jim</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>three</td>
      <td>1999</td>
      <td>green</td>
      <td>Kitty</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2000</td>
      <td>black</td>
      <td>Nancy</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2012</td>
      <td>white</td>
      <td>Amy</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



如果想查看DataFrame中的某一列，按照正常方式检索即可，所得结果是一个Series：


```python
Frame['name']
```




    one        Bob
    two        Jim
    three    Kitty
    four     Nancy
    five       Amy
    six       lily
    Name: name, dtype: object



如果想查看DataFrame中的某一行，可以通过位置或特殊属性loc进行选取。例如，我想查看Frame的第3行：


```python
Frame.loc['three']
```




    year        1999
    color      green
    name       Kitty
    country      NaN
    Name: three, dtype: object



DataFrame中，列的引用也是可以修改的。比如现在，country这一列是空的，我们可以对其进行赋值。且赋值为标量值或值数组均可，只需保持长度一致即可。

甚至可以将Series赋值给一个列。

如果被赋值的列不存在，将会生成一个新的列~


```python
Frame['country']='China'   # 赋值为标量
Frame
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
      <th>year</th>
      <th>color</th>
      <th>name</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>1995</td>
      <td>red</td>
      <td>Bob</td>
      <td>China</td>
    </tr>
    <tr>
      <td>two</td>
      <td>1997</td>
      <td>yellow</td>
      <td>Jim</td>
      <td>China</td>
    </tr>
    <tr>
      <td>three</td>
      <td>1999</td>
      <td>green</td>
      <td>Kitty</td>
      <td>China</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2000</td>
      <td>black</td>
      <td>Nancy</td>
      <td>China</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2012</td>
      <td>white</td>
      <td>Amy</td>
      <td>China</td>
    </tr>
    <tr>
      <td>six</td>
      <td>1999</td>
      <td>red</td>
      <td>lily</td>
      <td>China</td>
    </tr>
  </tbody>
</table>
</div>




```python
import numpy as np
Frame['country']=np.arange(6)  #赋值为数组时，长度保持一致.若长度不一致，将会报错
Frame
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
      <th>year</th>
      <th>color</th>
      <th>name</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>1995</td>
      <td>red</td>
      <td>Bob</td>
      <td>0</td>
    </tr>
    <tr>
      <td>two</td>
      <td>1997</td>
      <td>yellow</td>
      <td>Jim</td>
      <td>1</td>
    </tr>
    <tr>
      <td>three</td>
      <td>1999</td>
      <td>green</td>
      <td>Kitty</td>
      <td>2</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2000</td>
      <td>black</td>
      <td>Nancy</td>
      <td>3</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2012</td>
      <td>white</td>
      <td>Amy</td>
      <td>4</td>
    </tr>
    <tr>
      <td>six</td>
      <td>1999</td>
      <td>red</td>
      <td>lily</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
Frame['country']=pd.Series([-1,0,1],index=['one','two','five'])  # Series赋值，空缺的地方自动填补缺失值
Frame
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
      <th>year</th>
      <th>color</th>
      <th>name</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>1995</td>
      <td>red</td>
      <td>Bob</td>
      <td>-1.0</td>
    </tr>
    <tr>
      <td>two</td>
      <td>1997</td>
      <td>yellow</td>
      <td>Jim</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>three</td>
      <td>1999</td>
      <td>green</td>
      <td>Kitty</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2000</td>
      <td>black</td>
      <td>Nancy</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2012</td>
      <td>white</td>
      <td>Amy</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>six</td>
      <td>1999</td>
      <td>red</td>
      <td>lily</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
Frame['OO后']=Frame.year>=2000  # 在这条语句中，Frame['OO后']这一列是布尔值，后面是一个条件语句
Frame
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
      <th>year</th>
      <th>color</th>
      <th>name</th>
      <th>country</th>
      <th>OO后</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>1995</td>
      <td>red</td>
      <td>Bob</td>
      <td>-1.0</td>
      <td>False</td>
    </tr>
    <tr>
      <td>two</td>
      <td>1997</td>
      <td>yellow</td>
      <td>Jim</td>
      <td>0.0</td>
      <td>False</td>
    </tr>
    <tr>
      <td>three</td>
      <td>1999</td>
      <td>green</td>
      <td>Kitty</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2000</td>
      <td>black</td>
      <td>Nancy</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2012</td>
      <td>white</td>
      <td>Amy</td>
      <td>1.0</td>
      <td>True</td>
    </tr>
    <tr>
      <td>six</td>
      <td>1999</td>
      <td>red</td>
      <td>lily</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
del Frame['color']    # del 方法移除列
Frame
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
      <th>year</th>
      <th>name</th>
      <th>country</th>
      <th>OO后</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>one</td>
      <td>1995</td>
      <td>Bob</td>
      <td>-1.0</td>
      <td>False</td>
    </tr>
    <tr>
      <td>two</td>
      <td>1997</td>
      <td>Jim</td>
      <td>0.0</td>
      <td>False</td>
    </tr>
    <tr>
      <td>three</td>
      <td>1999</td>
      <td>Kitty</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
    <tr>
      <td>four</td>
      <td>2000</td>
      <td>Nancy</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <td>five</td>
      <td>2012</td>
      <td>Amy</td>
      <td>1.0</td>
      <td>True</td>
    </tr>
    <tr>
      <td>six</td>
      <td>1999</td>
      <td>lily</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



除了上述构造DataFrame的方法，还可以使用包含字典的嵌套字典、包含Series的字典等来构造，详情参见P133

### 5.1.3 索引对象 P135
pandas中的索引对象是用于存储轴标签和其它元数据的（例如轴名称或标签）。索引对象是不可变的，用户无法对其进行修改，这种不可变性也使得在多种数据结构中分享索引对象更加安全。

与Python集合不同，Pandas索引对象可以包含重复标签，根据重复标签进行筛选时，会选出所有重复标签对应的数据。
