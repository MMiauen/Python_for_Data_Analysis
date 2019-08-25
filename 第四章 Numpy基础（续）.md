## 4.2 通用函数

通用函数，又称ufunc，是一种在ndarray数据中进行逐元素操作的函数。P107

一元通用函数：abs\fabs\sqrt\square\exp\modf\...

二元通用函数：add\subtract\multiply\mod\divide\power...

## 4.3 使用数组进行面向数组编程

### 4.3.1 将条件逻辑作为数组操作

在Numpy中，可以利用数组表达式来代替显式循环，这种方法称为向量化。

np.where函数就是三元表达式(x if condition else y)的向量化版本。np.where(condition,elementX,elementY)


```python
xarr=np.array([1,2,3,4,5])
yarr=np.array(['a','b','c','d','e'])
condition=np.array([True,False,True,True,False])

result1=[(x if c else y) for x,y,c in zip(xarr,yarr,condition)]
print('result1:',result1)

result2=np.where(condition,xarr,yarr)
print('result2:',result2)
```

    result1: [1, 'b', 3, 4, 'e']
    result2: ['1' 'b' '3' '4' 'e']
    

np.where的第二个第三个参数并不一定要是数组，也可以是标量。例如你有一个随机生成的数组，你想将数组中正值替换为1，负值替换为-1，操作如下：


```python
arr=np.random.randn(3,3)
arr
```




    array([[ 1.24096447,  0.71165354,  0.49714955],
           [-2.10383327,  1.35545816, -1.58925675],
           [-0.41133194,  1.03281777,  1.74883479]])




```python
np.where(arr>0,1,-1)
```




    array([[ 1,  1,  1],
           [-1,  1, -1],
           [-1,  1,  1]])




```python
np.where(arr>0,1,arr) #仅将正值设为1，负值不变
```




    array([[ 1.        ,  1.        ,  1.        ],
           [-2.10383327,  1.        , -1.58925675],
           [-0.41133194,  1.        ,  1.        ]])



### 4.3.2 数学和统计方法

sum求和、mean数学平均、std标准差、min\max最值、argmin\argmax最值位置、cumsum从0开始元素累计和、cumprod从1开始元素累积积

mean\sum等函数还能接收一个可选参数axis，用于计算轴向统计值。np.sum(arr,axis=0) axis=0>>行,axis=1 >>列


```python
arr1=np.arange(15).reshape((3,5))
arr1
```




    array([[ 0,  1,  2,  3,  4],
           [ 5,  6,  7,  8,  9],
           [10, 11, 12, 13, 14]])




```python
np.mean(arr1) # >>arr.mean()
```




    7.0




```python
np.sum(arr1) # >>arr.sum()
```




    105




```python
arr2=np.array([1,2,3,4,5])
arr2.cumsum()
```




    array([ 1,  3,  6, 10, 15], dtype=int32)




```python
arr2.cumprod()
```




    array([  1,   2,   6,  24, 120], dtype=int32)



### 4.3.3 布尔值数组的方法

由于布尔值会被强制为1（TRUE）和0（FALSE），因此，sum可以计算出数组中TRUE的个数。

除此之外，介绍两个布尔数组中的重要方法：any和all。any>>检查数组中是否至少有一个TRUE，all>>检查数组是否全为TRUE

上述方法也适用于非布尔数组，所有非0元素都被当成TRUE处理


```python
arr=np.array([-1,2,3,-4,-5])
(arr>0).sum() #正值个数
```




    2




```python
arr1=np.array([True,False,False])
arr1.any()
```




    True




```python
arr1.all()
```




    False



### 4.3.4唯一值与其他集合逻辑

unique(x) 计算x的唯一值并排序

in1d(x,y) 检查x中的值是否在y中，并返回一个与x长度相等的布尔值数组

intersect1d(x,y) 计算x与y的交集，并排序

union1d(x,y) 计算x与y的并集，并排序


```python
names=np.array(['amy','bob','kitty','bob','dog','kitty'])
np.unique(names)
```




    array(['amy', 'bob', 'dog', 'kitty'], dtype='<U5')




```python
arr1=np.array([1,3,5,7,9])
arr2=np.array([3,5])
np.in1d(arr1,arr2)
```




    array([False,  True,  True, False, False])



## 4.4 使用数组进行文件输入与输出

Numpy可以实现文本文件或二进制文件的硬盘输入输出，但这里只讨论Numpy的内建二进制格式数据，因为大部分用户倾向于用pandas载入文本、表格数据。

np.save与np.load是高效存取硬盘数据的两大工具函数。数据存储时默认情况是未压缩的，且后缀为.npy


```python
arr=np.arange(10)
np.save('some_array',arr)
```


```python
np.load('some_array.npy')   #存储的时候可以不写后缀，但调用时要加上
```




    array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])



## 4.5线性代数

numpy.linalg拥有一个矩阵分解的标准函数集及其它常用函数，例如求逆、行列式求解等等 P117

from numpy.linalg import XXX(<--所需函数名称)

## 4.6伪随机数生成

 ### numpy.random中部分函数列表 P119
 seed 向随机数生成器传递随机状态种子（也就是生成随机数的起点）
 
 rand 从均匀分布中抽取样本
 
 randn 从均值0方差1的正太分布中抽取样本
 
 normal 从正态分布中抽取样本
 
 randint 由给定的从低到高的范围抽取随机整数

【补充知识】：关于Python之Random.randint()与numpy.random.randint()的区别

random.randint(a,b),生成随机数范围是[a,b]，例如random.randint(1,4)，会生成随机数1、2、3、4

np.random.randint(a,b)生成随机数范围是[a,b)，例如np.random.randint(1,4),会生成随机数1、2、3


```python
example=np.random.normal(size=(3,3))
example
```




    array([[-0.27429567, -0.45526409,  1.5719167 ],
           [-0.20651206,  0.00561679,  0.97706254],
           [-1.17662055,  0.53064991, -0.87184913]])



numpy.random中数据的生成函数公用了一个全局的随机数种子。为了避免全局状态，可以使用numpy.random.RandomState生成一个随机数生成器，使数据独立于其它随机数状态


```python
rng=np.random.RandomState(1234)
rng.randn(10)
```




    array([ 0.47143516, -1.19097569,  1.43270697, -0.3126519 , -0.72058873,
            0.88716294,  0.85958841, -0.6365235 ,  0.01569637, -2.24268495])



## 4.7 示例：随机漫步

一个简单的随机漫步：从0开始，步进为1或-1，且两种步进发生概率相等。下面使用内建random模块利用纯python实现一个1000步的随机漫步：


```python
import random
position=0        #起始位置
walk=[position]
steps=1000
for i in range(steps):
    step=1 if random.randint(0,1) else -1   # random.randint(0,1)会生成随机数0、1，
    position=position+step
    walk.append(position)
plt.plot(walk[:100])   #前100步的可视化
```




    [<matplotlib.lines.Line2D at 0xa1751f46a0>]




![png](output_40_1.png)


下面再使用np.random模块进行随机漫步，假设条件与上述例子相同。


```python
steps=1000
draws=np.random.randint(0,2,size=steps)  #生成随机数0、1,个数与步数相同
steps=np.where(draws>0,1,-1)
walk=steps.cumsum() #从0开始累积元素和
plt.plot(walk[:100])
```




    [<matplotlib.lines.Line2D at 0xa17523cc18>]




![png](output_42_1.png)



```python
walk.min()  
```




    -11




```python
walk.max()
```




    32




```python
#np.abs(walk)>=10   >>  漫步是否连续在同一方向走了10步 >> 返回一个布尔数组
(np.abs(walk)>=10).argmax()  #第一次走了10步或者-10步的位置，可以使用argmax计算，但是效率不高，因为它需要扫面整个数组
```




    201



### 一次性模拟多次随机漫步 P121


```python

```
