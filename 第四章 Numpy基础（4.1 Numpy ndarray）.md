# 第4章 NumPy基础：数组与向量化计算

NumPy,Numerical Python,是Python数值计算中最为重要的基础包，它的设计对于含有大量数组的数据结构非常有效。

Numpy算法库是用C语言编写的，它还提供了非常易用的C语言API，使得数据传递给用底层语言编写的外部类库，再将计算结果按NumPy数组返回变得简单。

NumPy数组的内存使用量也小于其它Python内建序列。

NumPy可以对全量数组进行复杂计算而不需要编写Python循环。

## 4.1 NumPy ndarray:多维数组对象

### 4.1.1 生成ndarry

生成数组最简单的方式就是使用array函数


```python
import numpy as np

data1=[6,7.5,8,0,1]
arr1=np.array(data1)   #列表转化为Numpy数组
arr1
```




    array([6. , 7.5, 8. , 0. , 1. ])




```python
data2=[[1,2,3],[4,5,6]]  #嵌套序列，将被转化为多维数组
arr2=np.array(data2)
arr2
```




    array([[1, 2, 3],
           [4, 5, 6]])



array.ndim >> 数组维度

array.shape >>数组形状

array.dtype >>数组数据类型  默认为浮点型float64

除了np.array,还有很多函数可以创建新数组。

例如，给定长度和形状后，zeros可创建全零数组，ones可创建全1数组，empty可创建无初始化数值的数组,full可创建生成指定数值的数组,eye可创建对角线全为1，其余全为0的NxN特征矩阵 


```python
np.zeros(5)
```




    array([0., 0., 0., 0., 0.])




```python
np.ones((2,3)) #shape为元组时，创建的是高维数组
```




    array([[1., 1., 1.],
           [1., 1., 1.]])




```python
np.empty((2,3,2))
```




    array([[[0., 0.],
            [0., 0.],
            [0., 0.]],
    
           [[0., 0.],
            [0., 0.],
            [0., 0.]]])




```python
np.full([3,2],9)  #np.full(shape,element)
```




    array([[9, 9],
           [9, 9],
           [9, 9]])




```python
np.eye(5)
```




    array([[1., 0., 0., 0., 0.],
           [0., 1., 0., 0., 0.],
           [0., 0., 1., 0., 0.],
           [0., 0., 0., 1., 0.],
           [0., 0., 0., 0., 1.]])




```python
np.arange(15) #arange是Python内建函数range的数组版
```




    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14])



补充：ones_like/zeros_like/empty_like/full_like 根据所给数组生成形状一模一样的的全0/全1/空/指定数值数组。


```python
np.ones_like(arr2) # arr2:shape(2,3)
```




    array([[1, 1, 1],
           [1, 1, 1]])



### 4.1.2 ndarry的数据类型

数据类型，即dtype,dtype是Numpy能够与其他系统数据灵活交互的原因。作为新手，只需关注数据的大类，如整数、浮点数、布尔值、字符串等。当需要在内存或硬盘作深入的读取操作时，则要深入了解。P93


```python
arr1=np.array([1,2,3],dtype=np.float64)
arr2=np.array([1,2,3],dtype=np.int32)
print('arr1 dtype:',arr1.dtype)
print('arr2 dtype:',arr2.dtype)
```

    arr1 dtype: float64
    arr2 dtype: int32
    

astype方法可以显式地转换数组的数据类型，且总是生成一个新数组，即使你传入的dtype与之前一样

除此之外，astype还可以使用另一数组的dtype属性


```python
arr1_after=arr1.astype(np.int16)
print('arr1 dtype after transfer:',arr1_after.dtype)
```

    arr1 dtype after transfer: int16
    


```python
arr2_after=arr2.astype(arr1.dtype)
print('arr2 dtypr after transfer:',arr2_after.dtype)
```

    arr2 dtypr after transfer: float64
    

### 4.1.3 Numpy数组算术

数组之所以重要是因为它允许你进行批量操作而无需任何for循环，Numpy用户称该特性为向量化。任何两个等尺寸数组之间的算术操作都运用了逐元素操作方式，同尺寸数组间的比较，会产生一个布尔值数组。不同尺寸的数组间操作，涉及广播特性。本书不必深入了解。


```python
arr=np.array([[1,2,3],[4,5,6],[7,8,9]])
arr
```




    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])




```python
arr*arr
```




    array([[ 1,  4,  9],
           [16, 25, 36],
           [49, 64, 81]])




```python
arr-arr
```




    array([[0, 0, 0],
           [0, 0, 0],
           [0, 0, 0]])




```python
1/arr
```




    array([[1.        , 0.5       , 0.33333333],
           [0.25      , 0.2       , 0.16666667],
           [0.14285714, 0.125     , 0.11111111]])




```python
arr2=np.array([[1,4,2],[4,7,5],[9,6,1]])
arr>arr2 
```




    array([[False, False,  True],
           [False, False,  True],
           [False,  True,  True]])



### 4.1.4基础索引与切片

Numpy中数组索引、切片方法与python内建列表一样。不同之处在于，Numpy数组任何修改都直接反映在原数组上，而并非重新复制一份数据再修改。这是由于Numpy特性决定的，Numpy被设计为适合处理非常大的数组，试想要是Numpy处理数据需要复制内容，得需要多大的内存

若还是想得到数组切片的拷贝而非原视图，必须显式地复制这个数组~ 例如：arr[5:8].copy()

对于高维数组，例如二维数组，每个索引值对应的元素不再是一个值，而是一个一维数组,取单个元素的方法也略有不同：


```python
arr2d=np.array([[1,2,3],[4,5,6],[7,8,9]])
print(arr2d[1])
print(arr2d[1][0])
print(arr2d[1,0])
```

    [4 5 6]
    4
    4
    

对于多维数组，省略后续索引值，返回对象将是降低一个维度的数组。例如：


```python
arr3d=np.array([[[1,2,3],[4,5,6]],[[7,8,9],[10,11,12]]])#  2x2x3
print('arr3d:',arr3d)
print('arr3d[1]:',arr3d[1])  # 2x3
print('arr3d[1,0]:',arr3d[1,0])
print('arr3d[1,0,0]:',arr3d[1,0,0])
```

    arr3d: [[[ 1  2  3]
      [ 4  5  6]]
    
     [[ 7  8  9]
      [10 11 12]]]
    arr3d[1]: [[ 7  8  9]
     [10 11 12]]
    arr3d[1,0]: [7 8 9]
    arr3d[1,0,0]: 7
    

数组的切片索引。[行，列],axis0 >> 行 ；axis1 >> 列


```python
arr2d=np.array([[1,2,3],[4,5,6],[7,8,9]])
arr2d
```




    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])




```python
arr2d[:2] #对行进行切片操作
```




    array([[1, 2, 3],
           [4, 5, 6]])




```python
arr2d[ :2,1:]
```




    array([[2, 3],
           [5, 6]])




```python
arr2d[ :2,1:]=99 #对切片表达式赋值时，整个切片都会被赋值
arr2d
```




    array([[ 1, 99, 99],
           [ 4, 99, 99],
           [ 7,  8,  9]])



### 4.1.5 布尔索引

例子：我们的数据是一组存在重复名字的数组 names，再生成一组正态分布的随机数据data，假设每个人名都和data中的一行数据对应，现在我们想找出所有与名字'Amy'对应的data行


```python
names=np.array(['Amy','Bob','Caty','Amy','Bob','Amy'])
data=np.random.randn(6,4)
print('names:',names)
print('data:',data)
```

    names: ['Amy' 'Bob' 'Caty' 'Amy' 'Bob' 'Amy']
    data: [[ 1.41672157  1.57524562 -1.12686827  0.94528684]
     [ 0.01971917 -0.29028038 -0.00651392 -0.08901394]
     [ 0.77032518  0.19346553 -0.4262715   1.89226109]
     [-1.20777517 -0.97856659 -0.33222289  0.5488082 ]
     [-1.29422936  2.30275341 -0.20656737  1.56412765]
     [-0.45505207  0.12599216 -0.47887613  0.76268814]]
    


```python
names=='Amy'  # names数组和字符串'Amy'会产生一个布尔值数组。注意：布尔值数组长度必须和数组轴索引长度一致
```




    array([ True, False, False,  True, False,  True])




```python
data[names=='Amy']  #索引数组时会传入布尔值数据
```




    array([[ 1.41672157,  1.57524562, -1.12686827,  0.94528684],
           [-1.20777517, -0.97856659, -0.33222289,  0.5488082 ],
           [-0.45505207,  0.12599216, -0.47887613,  0.76268814]])



若想选择'Amy'之外的其他数据，可以使用：data[names!='Amy'] 或者 data[~(names='Amy')] 

当要选择多个名字时，需要使用数学操作符&（和），|(或)，例如 data[(names='Amy')|(names='Bob')]

### 4.1.6 神奇索引

神奇索引与是Numpy中的术语，用于使用整数数组进行数据索引。神奇索引与切片不同，它总是将数据复制到一个新的数组中。


```python
arr=np.array([[0,0,0],[1,1,1],[2,2,2],[3,3,3],[4,4,4],[5,5,5],[6,6,6],[7,7,7]])
arr
```




    array([[0, 0, 0],
           [1, 1, 1],
           [2, 2, 2],
           [3, 3, 3],
           [4, 4, 4],
           [5, 5, 5],
           [6, 6, 6],
           [7, 7, 7]])




```python
arr[[3,6,1]]   #数组[3,6,1]中的数字是索引噢
```




    array([[3, 3, 3],
           [6, 6, 6],
           [1, 1, 1]])




```python
arr[[-1,-3,-5]] # 还可以使用负索引
```




    array([[7, 7, 7],
           [5, 5, 5],
           [3, 3, 3]])




```python
arr1=np.arange(32).reshape((8,4))
arr1
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11],
           [12, 13, 14, 15],
           [16, 17, 18, 19],
           [20, 21, 22, 23],
           [24, 25, 26, 27],
           [28, 29, 30, 31]])




```python
arr1[[1,5,7,2],[0,3,1,2]]  #选中元素(1,0),(5,3),(7,1),(2,2)
```




    array([ 4, 23, 29, 10])



神奇索引所得结果与我们想象并不相同，若我们希望选中行列的子集所形成的矩形区域。实现方式如下：


```python
arr1[[1,5,7,2]][:,[0,3,1,2]]
```




    array([[ 4,  7,  5,  6],
           [20, 23, 21, 22],
           [28, 31, 29, 30],
           [ 8, 11,  9, 10]])



### 4.1.7 数组转置与换轴

转置：arr.T ; 

内积：np.dot

换轴：transpose\swapaxes P105


```python
arr=np.arange(8).reshape((2,4))
arr
```




    array([[0, 1, 2, 3],
           [4, 5, 6, 7]])




```python
arr.T
```




    array([[0, 4],
           [1, 5],
           [2, 6],
           [3, 7]])




```python
np.dot(arr.T,arr)
```




    array([[16, 20, 24, 28],
           [20, 26, 32, 38],
           [24, 32, 40, 48],
           [28, 38, 48, 58]])


