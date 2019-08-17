# 第一章 Ipython及Jupyter notebook

## 1.1 Ipython

Ipython部分之后补充，先用jupyter.

# 1.2 jupyter

Jupyter Notebook 原名Ipython Notebook，这是因为默认情况下Jupyter Notebook使用python内核。Jupyter Notebook本质是一个web应用程序，便于创建和共享程序文档，支持实时代码、数学方程、可视乎和markdown。在Jupyter Notebook中，代码可以实时生成图像、视频、LaTeX 和Javascript。


### 1.2.1 快捷键 

Jupyter在顶部菜单提供了快捷键列表：Help>Keyboard Shortcuts，几个常用的快捷键：
ctrl + Enter > 运行整个单元格
Alt + Enter  > 运行整个单元格并在下方添加新输入行
ctrl + shift + F >打开命令选项板

### 1.2.2 内省

？：在一个变量名前后使用问号，可以显示关于该对象的概要信息

？：在一个函数前后使用问号，可以显示文档字符串（也就是三引号里的内容）

？？：显示函数源代码

？的终极用途：把一些字符和通配符（*）结合在一起，再加问号，会显示所有匹配通配符表达式的命名


```python
a=[1,2,3]
```


```python
a?
```

![image.png](attachment:image.png)


```python
def sum_numbers(a,b):
    '''add two numbers together
    returns
    the_sum:type of arguments'''
    return a+b
```


```python
sum_numbers?
```

![image.png](attachment:image.png)


```python
sum_numbers??
```

![image.png](attachment:image.png)


```python
import numpy as np
np.*load*?      # 得到Numpy顶层函数中包含load的所有函数名列表
```

![image.png](attachment:image.png)

### 1.2.3 matplotlib集成

初次使用Jupyter Notebook作图时，系统里没有matplotlib模块，若使用pip安装需要在命令语句前面加一个感叹号，这相当于告诉Jupyter Notebook把这条命令当作shell命令来执行，通过%matplotlib inline激活即可正常使用~


```python
!pip install matplotlib
```

    Collecting matplotlib
      Downloading https://files.pythonhosted.org/packages/1a/c0/69e3f695d7384012e90be1e16570c08953baae00fd98094179ef87c7d5a2/matplotlib-3.1.1-cp37-cp37m-win_amd64.whl (9.1MB)
    Collecting cycler>=0.10 (from matplotlib)
      Downloading https://files.pythonhosted.org/packages/f7/d2/e07d3ebb2bd7af696440ce7e754c59dd546ffe1bbe732c8ab68b9c834e61/cycler-0.10.0-py2.py3-none-any.whl
    Collecting kiwisolver>=1.0.1 (from matplotlib)
      Downloading https://files.pythonhosted.org/packages/c6/ea/e5474014a13ab2dcb5056608e0716c600c3d8a8bcffb10ed55ccd6a42eb0/kiwisolver-1.1.0-cp37-none-win_amd64.whl (57kB)
    Collecting pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 (from matplotlib)
      Downloading https://files.pythonhosted.org/packages/11/fa/0160cd525c62d7abd076a070ff02b2b94de589f1a9789774f17d7c54058e/pyparsing-2.4.2-py2.py3-none-any.whl (65kB)
    Requirement already satisfied: python-dateutil>=2.1 in d:\python\lib\site-packages (from matplotlib) (2.8.0)
    Requirement already satisfied: numpy>=1.11 in c:\users\administrator.sc-201903122001\appdata\roaming\python\python37\site-packages (from matplotlib) (1.17.0)
    Requirement already satisfied: six in d:\python\lib\site-packages (from cycler>=0.10->matplotlib) (1.12.0)
    Requirement already satisfied: setuptools in d:\python\lib\site-packages (from kiwisolver>=1.0.1->matplotlib) (40.8.0)
    Installing collected packages: cycler, kiwisolver, pyparsing, matplotlib
    Successfully installed cycler-0.10.0 kiwisolver-1.1.0 matplotlib-3.1.1 pyparsing-2.4.2
    


```python
%matplotlib inline
```


```python
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif']=['simHei']
plt.rcParams['axes.unicode_minus']=False
plt.plot(np.random.randn(50).cumsum())
```




    [<matplotlib.lines.Line2D at 0x51b6aa9da0>]




![png](output_23_1.png)


### 1.2.4 Jupyter魔术命令

magic函数主要包含两大类：一类是行魔法（line magic)前缀为%，一类是单元魔法（cell magic)前缀为%%


%ismagic >> 用于查询当前可用的所有魔法命令。前面所学问号的功能在这里同样适用：魔法命令+？ >> 显示魔法命令的说明Docstring


%matplotlib inline >> 使用matplotlib绘图时，图片嵌入在jupyter notebook里，不以单独窗口显示

%run >> %run file_name.py 表示运行一个py文件

%load >> %load file_name.py 表示加载py文件里的内容

%%writefile >> %%writefile file_name.py 表示在jupyter notebook中创建一个py文件，后面cell的内容为文件内容


