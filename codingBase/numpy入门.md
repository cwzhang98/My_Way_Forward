# 基础知识

NumPy的主要对象是同构多维数组。它是一个元素表（通常是数字），所有类型都相同，由非负整数元组索引。在NumPy中每个维度称为一个*轴* 。例如[1,2,3]具有一个轴。

## ndarray

numpy的数组类为ndarray，别名为`array`，`numpy.array`这与标准Python库类不同`array.array`，后者只处理一维数组并提供较少的功能。`ndarray`对象更重要的属性是：

- **ndarray.ndim** - 数组的轴（维度）的个数。返回数字。
- **ndarray.shape** - 数组的维度。这是一个整数的元组，表示每个维度中数组的大小。对于有 *n* 行和 *m*列的矩阵，`shape` 将是 `(n,m)`。因此，`shape` 元组的长度就是rank或维度的个数 `ndim`。
- **ndarray.size** - 数组元素的总数。这等于 `shape` 的元素的乘积。
- **ndarray.dtype** - 一个描述数组中元素数据类型的对象。可以使用标准的Python类型创建或指定dtype。另外NumPy提供它自己的类型。例如numpy.int32、numpy.int16和numpy.float64。
- **ndarray.itemsize** - 数组中每个元素的字节大小。例如，元素为 `float64` 类型的数组的 `itemsize`为8（=64/8），而 `complex32` 类型的数组的 `itemsize` 为4（=32/8）。它等于 `ndarray.dtype.itemsize` 。
- **ndarray.data** - 该缓冲区包含数组的实际元素。通常，我们不需要使用此属性，因为我们将使用索引访问数组中的元素。

```python
>>> import numpy as np
#arange(num)生成0-num-1的整数的nadarray，reshape(row,col)重整数组结构
>>> a = np.arange(15).reshape(3, 5)
>>> a
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
>>> a.shape
(3, 5)
>>> a.ndim
2
>>> a.dtype.name
'int64'
>>> a.itemsize
8
>>> a.size
15
>>> type(a)
<type 'numpy.ndarray'>
>>> b = np.array([6, 7, 8])
>>> b
array([6, 7, 8])
>>> type(b)
<type 'numpy.ndarray'>
```

## 数组创建

 可以从常规Python列表或元组中创建数组。类型会自动推导。

```python
import numpy as np
a = np.array([2,3,4])
```

array会自动将列表，元组等序列的嵌套形式转化为多维数组。

```python
>>> b = np.array([(1.5,2,3), (4,5,6)])
>>> b
array([[ 1.5,  2. ,  3. ],
       [ 4. ,  5. ,  6. ]])
```

指定数据类型：

```python
c = np.array( [ [1,2], [3,4] ], dtype=complex )
'''
array([[ 1.+0.j,  2.+0.j],
       [ 3.+0.j,  4.+0.j]])
'''
```

## 打印数组

将一维数组打印为行，将二维数据打印为矩阵，将三维数据打印为矩数组表。

```python
>>> a = np.arange(6)                         # 1d array
>>> print(a)
[0 1 2 3 4 5]
>>>
>>> b = np.arange(12).reshape(4,3)           # 2d array
>>> print(b)
[[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]
>>>
>>> c = np.arange(24).reshape(2,3,4)         # 3d array
>>> print(c)
[[[ 0  1  2  3]
  [ 4  5  6  7]
  [ 8  9 10 11]]
 [[12 13 14 15]
  [16 17 18 19]
  [20 21 22 23]]]
```

如果数组太大而无法打印，NumPy会只打印开头和结尾，中间部分用省略号占位。

```python
>>> print(np.arange(10000))
[   0    1    2 ..., 9997 9998 9999]
>>>
>>> print(np.arange(10000).reshape(100,100))
[[   0    1    2 ...,   97   98   99]
 [ 100  101  102 ...,  197  198  199]
 [ 200  201  202 ...,  297  298  299]
 ...,
 [9700 9701 9702 ..., 9797 9798 9799]
 [9800 9801 9802 ..., 9897 9898 9899]
 [9900 9901 9902 ..., 9997 9998 9999]]
```

> 可以使用np.set_printoptions(threshold=sys.maxsize)更改打印选项，强制打印整个数组。

## 基本操作

算术运算例如算数运算符和比较运算符会运用到每个元素上。

```python
a = np.array( [20,30,40,50] )
b = np.arange( 4 )
c = a-b
# c的结果为array([20, 29, 38, 47])
```

> 需要注意的是，乘积运算符`*`在NumPy数组中按元素进行运算。矩阵乘积可以使用`@`运算符（在python> = 3.5中）或`dot`函数或方法执行

```python
>>> A @ B                       # matrix product
array([[5, 4],
       [3, 4]])
>>> A.dot(B)                    # another matrix product
array([[5, 4],
       [3, 4]])
```

二元运算符会直接更改被操作的矩阵数组而不会创建新矩阵数组。

```python
>>> a = np.ones((2,3), dtype=int)
>>> b = np.random.random((2,3))
>>> a *= 3
>>> a
array([[3, 3, 3],
       [3, 3, 3]])
```

不同数据类型的数组的算数运算，数据类型会向上转换。

```python
>>> a = np.ones(3, dtype=np.int32)
>>>b = np.linspace(0,pi,3)#linspace(start,end, num)在start和end之间均匀生成num个数据
>>> b.dtype.name
'float64'
>>> c = a+b
>>> c
array([ 1.        ,  2.57079633,  4.14159265])
>>> c.dtype.name
'float64'
>>> d = np.exp(c*1j)
>>> d
array([ 0.54030231+0.84147098j, -0.84147098+0.54030231j,
       -0.54030231-0.84147098j])
>>> d.dtype.name
'complex128'
```

ndarray类的一元操作：

- array.sum()求和
- array.min()求最小值
- array.max()求最大值

通过指定`axis`参数，可以沿指定轴应用操作，例如：

```python
b = np.arange(12).reshape(3,4)
b.sum(axis=0)
```

## 通函数

NumPy提供熟悉的数学函数，例如sin，cos和exp。在NumPy中，这些被称为“通函数”（`ufunc`）。在NumPy中，这些函数在数组上按元素进行运算，产生一个数组作为输出。

> 另见这些通函数
>
> [`all`](https://numpy.org/devdocs/reference/generated/numpy.all.html#numpy.all)， [`any`](https://numpy.org/devdocs/reference/generated/numpy.any.html#numpy.any)， [`apply_along_axis`](https://numpy.org/devdocs/reference/generated/numpy.apply_along_axis.html#numpy.apply_along_axis)， [`argmax`](https://numpy.org/devdocs/reference/generated/numpy.argmax.html#numpy.argmax)， [`argmin`](https://numpy.org/devdocs/reference/generated/numpy.argmin.html#numpy.argmin)， [`argsort`](https://numpy.org/devdocs/reference/generated/numpy.argsort.html#numpy.argsort)，[`average`](https://numpy.org/devdocs/reference/generated/numpy.average.html#numpy.average)， [`bincount`](https://numpy.org/devdocs/reference/generated/numpy.bincount.html#numpy.bincount)， [`ceil`](https://numpy.org/devdocs/reference/generated/numpy.ceil.html#numpy.ceil)， [`clip`](https://numpy.org/devdocs/reference/generated/numpy.clip.html#numpy.clip)， [`conj`](https://numpy.org/devdocs/reference/generated/numpy.conj.html#numpy.conj)， [`corrcoef`](https://numpy.org/devdocs/reference/generated/numpy.corrcoef.html#numpy.corrcoef)， [`cov`](https://numpy.org/devdocs/reference/generated/numpy.cov.html#numpy.cov)，[`cross`](https://numpy.org/devdocs/reference/generated/numpy.cross.html#numpy.cross)， [`cumprod`](https://numpy.org/devdocs/reference/generated/numpy.cumprod.html#numpy.cumprod)， [`cumsum`](https://numpy.org/devdocs/reference/generated/numpy.cumsum.html#numpy.cumsum)， [`diff`](https://numpy.org/devdocs/reference/generated/numpy.diff.html#numpy.diff)， [`dot`](https://numpy.org/devdocs/reference/generated/numpy.dot.html#numpy.dot)， [`floor`](https://numpy.org/devdocs/reference/generated/numpy.floor.html#numpy.floor)， [`inner`](https://numpy.org/devdocs/reference/generated/numpy.inner.html#numpy.inner)， [`lexsort`](https://numpy.org/devdocs/reference/generated/numpy.lexsort.html#numpy.lexsort)， [`max`](https://docs.python.org/dev/library/functions.html#max)， [`maximum`](https://numpy.org/devdocs/reference/generated/numpy.maximum.html#numpy.maximum)， [`mean`](https://numpy.org/devdocs/reference/generated/numpy.mean.html#numpy.mean)， [`median`](https://numpy.org/devdocs/reference/generated/numpy.median.html#numpy.median)， [`min`](https://docs.python.org/dev/library/functions.html#min)， [`minimum`](https://numpy.org/devdocs/reference/generated/numpy.minimum.html#numpy.minimum)， [`nonzero`](https://numpy.org/devdocs/reference/generated/numpy.nonzero.html#numpy.nonzero)， [`outer`](https://numpy.org/devdocs/reference/generated/numpy.outer.html#numpy.outer)， [`prod`](https://numpy.org/devdocs/reference/generated/numpy.prod.html#numpy.prod)， [`re`](https://docs.python.org/dev/library/re.html#module-re)， [`round`](https://docs.python.org/dev/library/functions.html#round)， [`sort`](https://numpy.org/devdocs/reference/generated/numpy.sort.html#numpy.sort)， [`std`](https://numpy.org/devdocs/reference/generated/numpy.std.html#numpy.std)， [`sum`](https://numpy.org/devdocs/reference/generated/numpy.sum.html#numpy.sum)， [`trace`](https://numpy.org/devdocs/reference/generated/numpy.trace.html#numpy.trace)， [`transpose`](https://numpy.org/devdocs/reference/generated/numpy.transpose.html#numpy.transpose)， [`var`](https://numpy.org/devdocs/reference/generated/numpy.var.html#numpy.var)， [`vdot`](https://numpy.org/devdocs/reference/generated/numpy.vdot.html#numpy.vdot)， [`vectorize`](https://numpy.org/devdocs/reference/generated/numpy.vectorize.html#numpy.vectorize)， [`where`](https://numpy.org/devdocs/reference/generated/numpy.where.html#numpy.where)

## 索引、切片和迭代

### 一维数组

**一维**的数组可以进行索引、切片和迭代操作的，就像列表和其他Python序列类型一样。

### 多维数组

**多维**的数组每个轴可以有一个索引。这些索引以逗号分隔的元组给出。

```python
>>> def f(x,y):
...     return 10*x+y
...
>>> b = np.fromfunction(f,(5,4),dtype=int)
>>> b
array([[ 0,  1,  2,  3],
       [10, 11, 12, 13],
       [20, 21, 22, 23],
       [30, 31, 32, 33],
       [40, 41, 42, 43]])
>>> b[2,3]
23
>>> b[0:5, 1]                       # each row in the second column of b
array([ 1, 11, 21, 31, 41])
>>> b[ : ,1]                        # equivalent to the previous example
array([ 1, 11, 21, 31, 41])
>>> b[1:3, : ]                      # each column in the second and third row of b
array([[10, 11, 12, 13],
       [20, 21, 22, 23]])
```

当提供的索引少于轴的数量时，缺失的索引被认为是完整的切片。

`b[i]` 方括号中的表达式 `i` 被视为后面紧跟着 `:` 的多个实例，用于表示剩余轴。NumPy也允许你使用三个点写为 `b[i,...]`。

三个点（ `...` ）表示产生完整索引元组所需的冒号。例如，如果 `x` 是rank为5的数组（即，它具有5个轴），则：

- `x[1,2,...]` 相当于 `x[1,2,:,:,:]`，
- `x[...,3]` 等效于 `x[:,:,:,:,3]`
- `x[4,...,5,:]` 等效于 `x[4,:,:,5,:]`。

### 迭代器

对数组中的每个元素执行操作，可以使用`flat`属性，该属性是数组的所有元素的迭代器：

```python
for element in b.flat:
    print(element)
```

- newaxis()用于函数切片中，创造新的轴
- ndenumerate()，多维迭代器，以元组下标迭代所有元素。

```python
a = np.array([[1, 2], [3, 4]])
for index, x in np.ndenumerate(a):
    print(index, x)
'''
(0, 0) 1
(0, 1) 2
(1, 0) 3
(1, 1) 4
'''

```

- Indices() 作用是返回一个给定形状数组的序号网格数组，可以用于提取数组元素或对数组进行切片使用。

## 形状操作

### 改变形状

以下三个命令都会回一个修改后的数组，但不会更改原始数组：

- ravel() 返回一维数组。
- reshape() 返回重新修改形状后的数组，参数为新数组形状。
- array.T 数组转置。
- resize()作用与reshape类似，不过resize会修改数组本身。

### 堆叠数组

- 行堆叠vstack(a, b)
- 列堆叠hstack(a, b)

- colunm_stack(a, b), 对二维数组的操作与hstack一致。对一维数组的操作，会把一维数组的元素按列优先生成二维数组。

- row_stack(a, b)类似

`r_`和`c_`在默认行为上类似于`vstack`和`hstack`, 不同的是，允许使用范围操作符,用于给出要连接的轴的编号。

```python
>>> np.r_[1:4,0,4]
array([1, 2, 3, 0, 4])
```

### 拆分数组

- hsplit(array, num)，拆分列(即沿行拆分)
- vsplit(array, num)，拆分行(即沿列拆分)
- array_split(array, num,axis=0), 指定拆分的列

## 拷贝和视图

当计算和操作数组时，有时会将数据复制到新数组中，有时则不会。这通常是初学者混淆的根源。有三种情况：

### 完全不复制

- 简单分配（赋值）不会复制数组对象或其数据。

- Python将可变对象作为引用传递，因此函数调用不会复制。

### 视图或浅拷贝

不同的数组对象可以共享相同的数据。该`view`方法创建一个查看相同数据的新数组对象。

`c = a.view()`

新数组对象有如下性质：

- c和a是不同的对象，但c的数据是a的数据的引用，即使改变c的shape。a和c数据也是互相引用的。所以`c is a`返回false，`c.base is a`返回true，c.flags.owndata为false。

> 切片数组返回的是一个视图

```python
>>> s = a[ : , 1:3]     # spaces added for clarity; could also be written "s = a[:,1:3]"
>>> s[:] = 10           # s[:] is a view of s. Note the difference between s=10 and s[:]=10
>>> a
array([[   0,   10,   10,    3],
       [1234,   10,   10,    7],
       [   8,   10,   10,   11]])
```

### 深拷贝

`copy`生成数组及数据的完整副本。

如果不再需要原始数组，则应在切片后调用 `copy`。例如，假设a是一个巨大的中间结果，最终结果b只包含a的一小部分，那么在用切片构造b时应该做一个深拷贝：

```python
>>> a = np.arange(int(1e8))
>>> b = a[:100].copy()
>>> del a  # the memory of ``a`` can be released.
```

## 常用函数

有关完整列表，请参阅[参考手册](https://www.numpy.org.cn/reference/)里的[常用API](https://www.numpy.org.cn/reference/routines/)。

- **数组的创建（Array Creation）** - [arange](https://numpy.org/devdocs/reference/generated/numpy.arange.html#numpy.arange), [array](https://numpy.org/devdocs/reference/generated/numpy.array.html#numpy.array), [copy](https://numpy.org/devdocs/reference/generated/numpy.copy.html#numpy.copy), [empty](https://numpy.org/devdocs/reference/generated/numpy.empty.html#numpy.empty), [empty_like](https://numpy.org/devdocs/reference/generated/numpy.empty_like.html#numpy.empty_like), [eye](https://numpy.org/devdocs/reference/generated/numpy.eye.html#numpy.eye), [fromfile](https://numpy.org/devdocs/reference/generated/numpy.fromfile.html#numpy.fromfile), [fromfunction](https://numpy.org/devdocs/reference/generated/numpy.fromfunction.html#numpy.fromfunction), [identity](https://numpy.org/devdocs/reference/generated/numpy.identity.html#numpy.identity), [linspace](https://numpy.org/devdocs/reference/generated/numpy.linspace.html#numpy.linspace), [logspace](https://numpy.org/devdocs/reference/generated/numpy.logspace.html#numpy.logspace), [mgrid](https://numpy.org/devdocs/reference/generated/numpy.mgrid.html#numpy.mgrid), [ogrid](https://numpy.org/devdocs/reference/generated/numpy.ogrid.html#numpy.ogrid), [ones](https://numpy.org/devdocs/reference/generated/numpy.ones.html#numpy.ones), [ones_like](https://numpy.org/devdocs/reference/generated/numpy.ones_like.html#numpy.ones_like), [zeros](https://numpy.org/devdocs/reference/generated/numpy.zeros.html#numpy.zeros), [zeros_like](https://numpy.org/devdocs/reference/generated/numpy.zeros_like.html#numpy.zeros_like)
- **转换和变换（Conversions）** - [ndarray.astype](https://numpy.org/devdocs/reference/generated/numpy.ndarray.astype.html#numpy.ndarray.astype), [atleast_1d](https://numpy.org/devdocs/reference/generated/numpy.atleast_1d.html#numpy.atleast_1d), [atleast_2d](https://numpy.org/devdocs/reference/generated/numpy.atleast_2d.html#numpy.atleast_2d), [atleast_3d](https://numpy.org/devdocs/reference/generated/numpy.atleast_3d.html#numpy.atleast_3d), [mat](https://numpy.org/devdocs/reference/generated/numpy.mat.html#numpy.mat)
- **操纵术（Manipulations）** - [array_split](https://numpy.org/devdocs/reference/generated/numpy.array_split.html#numpy.array_split), [column_stack](https://numpy.org/devdocs/reference/generated/numpy.column_stack.html#numpy.column_stack), [concatenate](https://numpy.org/devdocs/reference/generated/numpy.concatenate.html#numpy.concatenate), [diagonal](https://numpy.org/devdocs/reference/generated/numpy.diagonal.html#numpy.diagonal), [dsplit](https://numpy.org/devdocs/reference/generated/numpy.dsplit.html#numpy.dsplit), [dstack](https://numpy.org/devdocs/reference/generated/numpy.dstack.html#numpy.dstack), [hsplit](https://numpy.org/devdocs/reference/generated/numpy.hsplit.html#numpy.hsplit), [hstack](https://numpy.org/devdocs/reference/generated/numpy.hstack.html#numpy.hstack), [ndarray.item](https://numpy.org/devdocs/reference/generated/numpy.ndarray.item.html#numpy.ndarray.item), [newaxis](https://www.numpy.org.cn/reference/constants.html#numpy.newaxis), [ravel](https://numpy.org/devdocs/reference/generated/numpy.ravel.html#numpy.ravel), [repeat](https://numpy.org/devdocs/reference/generated/numpy.repeat.html#numpy.repeat), [reshape](https://numpy.org/devdocs/reference/generated/numpy.reshape.html#numpy.reshape), [resize](https://numpy.org/devdocs/reference/generated/numpy.resize.html#numpy.resize), [squeeze](https://numpy.org/devdocs/reference/generated/numpy.squeeze.html#numpy.squeeze), [swapaxes](https://numpy.org/devdocs/reference/generated/numpy.swapaxes.html#numpy.swapaxes), [take](https://numpy.org/devdocs/reference/generated/numpy.take.html#numpy.take), [transpose](https://numpy.org/devdocs/reference/generated/numpy.transpose.html#numpy.transpose), [vsplit](https://numpy.org/devdocs/reference/generated/numpy.vsplit.html#numpy.vsplit), [vstack](https://numpy.org/devdocs/reference/generated/numpy.vstack.html#numpy.vstack)
- **询问（Questions）** - [all](https://numpy.org/devdocs/reference/generated/numpy.all.html#numpy.all), [any](https://numpy.org/devdocs/reference/generated/numpy.any.html#numpy.any), [nonzero](https://numpy.org/devdocs/reference/generated/numpy.nonzero.html#numpy.nonzero), [where](https://numpy.org/devdocs/reference/generated/numpy.where.html#numpy.where),
- **顺序（Ordering）** - [argmax](https://numpy.org/devdocs/reference/generated/numpy.argmax.html#numpy.argmax), [argmin](https://numpy.org/devdocs/reference/generated/numpy.argmin.html#numpy.argmin), [argsort](https://numpy.org/devdocs/reference/generated/numpy.argsort.html#numpy.argsort), [max](https://docs.python.org/dev/library/functions.html#max), [min](https://docs.python.org/dev/library/functions.html#min), [ptp](https://numpy.org/devdocs/reference/generated/numpy.ptp.html#numpy.ptp), [searchsorted](https://numpy.org/devdocs/reference/generated/numpy.searchsorted.html#numpy.searchsorted), [sort](https://numpy.org/devdocs/reference/generated/numpy.sort.html#numpy.sort)
- **操作（Operations）** - [choose](https://numpy.org/devdocs/reference/generated/numpy.choose.html#numpy.choose), [compress](https://numpy.org/devdocs/reference/generated/numpy.compress.html#numpy.compress), [cumprod](https://numpy.org/devdocs/reference/generated/numpy.cumprod.html#numpy.cumprod), [cumsum](https://numpy.org/devdocs/reference/generated/numpy.cumsum.html#numpy.cumsum), [inner](https://numpy.org/devdocs/reference/generated/numpy.inner.html#numpy.inner), [ndarray.fill](https://numpy.org/devdocs/reference/generated/numpy.ndarray.fill.html#numpy.ndarray.fill), [imag](https://numpy.org/devdocs/reference/generated/numpy.imag.html#numpy.imag), [prod](https://numpy.org/devdocs/reference/generated/numpy.prod.html#numpy.prod), [put](https://numpy.org/devdocs/reference/generated/numpy.put.html#numpy.put), [putmask](https://numpy.org/devdocs/reference/generated/numpy.putmask.html#numpy.putmask), [real](https://numpy.org/devdocs/reference/generated/numpy.real.html#numpy.real), [sum](https://numpy.org/devdocs/reference/generated/numpy.sum.html#numpy.sum)
- **基本统计（Basic Statistics）** - [cov](https://numpy.org/devdocs/reference/generated/numpy.cov.html#numpy.cov), [mean](https://numpy.org/devdocs/reference/generated/numpy.mean.html#numpy.mean), [std](https://numpy.org/devdocs/reference/generated/numpy.std.html#numpy.std), [var](https://numpy.org/devdocs/reference/generated/numpy.var.html#numpy.var)
- **基本线性代数（Basic Linear Algebra）** - [cross](https://numpy.org/devdocs/reference/generated/numpy.cross.html#numpy.cross), [dot](https://numpy.org/devdocs/reference/generated/numpy.dot.html#numpy.dot), [outer](https://numpy.org/devdocs/reference/generated/numpy.outer.html#numpy.outer), [linalg.svd](https://numpy.org/devdocs/reference/generated/numpy.linalg.svd.html#numpy.linalg.svd), [vdot](https://numpy.org/devdocs/reference/generated/numpy.vdot.html#numpy.vdot)

## Less基础

### 广播（Broadcasting）规则

NumPy 操作通常在逐个元素的基础上在数组对上完成。在最简单的情况下，两个数组必须具有完全相同的形状，如下例所示：

```python
>>> a = np.array([1.0, 2.0, 3.0])
>>> b = np.array([2.0, 2.0, 2.0])
>>> a * b
array([ 2.,  4.,  6.])
```

当数组的形状满足某些约束时，NumPy的广播规则放宽了这种约束。当一个数组和一个标量值在一个操作中组合时，会发生最简单的广播示例：

```python
>>> a = np.array([1.0, 2.0, 3.0])
>>> b = 2.0
>>> a * b
array([ 2.,  4.,  6.])
```

#### 一般广播规则

在两个数组上运行时，NumPy会逐元素地比较它们的形状。它从尾轴尺寸开始，并向前发展。

当比较的任何一个尺寸为1时，使用另一个尺寸。例如：

有`A`,`B`两个数组，从尾部开始，若有`A`某个维度的尺寸为1，则广播规则会把这个维度的尺寸拉伸到与`B`相同。

```python
A      (4d array):  8 x 1 x 6 x 1
B      (3d array):      7 x 1 x 5
Result (4d array):  8 x 7 x 6 x 5
```

广播提供了一种方便的方式来获取两个数组的外积（或任何其他外部操作）。以下示例显示了两个1-d数组的外积操作：

```python
>>> a = np.array([0.0, 10.0, 20.0, 30.0])
>>> b = np.array([1.0, 2.0, 3.0])
>>> a[:, np.newaxis] + b
'''
首先a增加一条轴，a被拉伸为4*1的矩阵，再在广播规则下与b相加
'''
array([[  1.,   2.,   3.],
       [ 11.,  12.,  13.],
       [ 21.,  22.,  23.],
       [ 31.,  32.,  33.]])
```

## 索引技巧

numpy支持python中的所有索引功能，如通过整数和切片进行索引。除此之外，数组可以由整数数组和布尔数组索引。

### 使用索引数组进行索引

```python
>>> a = np.arange(12)**2                       # the first 12 square numbers
>>> i = np.array( [ 1,1,3,8,5 ] )              # an array of indices
>>> a[i]                                       # the elements of a at the positions i
array([ 1,  1,  9, 64, 25])
>>>
>>> j = np.array( [ [ 3, 4], [ 9, 7 ] ] )      # a bidimensional array of indices
>>> a[j]                                       # the same shape as j
array([[ 9, 16],
       [81, 49]])
```

> 当索引数组是多维的时候，单个索引子数组会生成新的数组，同时整体会增加一个维度。详细如下：

```python
>>> palette = np.array( [ [0,0,0],                # black
...                       [255,0,0],              # red
...                       [0,255,0],              # green
...                       [0,0,255],              # blue
...                       [255,255,255] ] )       # white
>>> image = np.array( [ [ 0, 1, 2, 0 ],           # each value corresponds to a color in the palette
...                     [ 0, 3, 4, 0 ]  ] )
>>> palette[image]                            # the (2,4,3) color image
array(
  [
    [
       [  0,   0,   0],
       [255,   0,   0],
       [  0, 255,   0],
       [  0,   0,   0]
    ],
    [
      [  0,   0,   0],
      [  0,   0, 255],
      [255, 255, 255],
      [  0,   0,   0]
    ]
  ]
)
```

还可以为多个维度提供索引。每个维度的索引数组必须具有相同的形状。

```python
>>> a = np.arange(12).reshape(3,4)
>>> a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>> i = np.array( [ [0,1],                        # indices for the first dim of a
...                 [1,2] ] )
>>> j = np.array( [ [2,1],                        # indices for the second dim
...                 [3,3] ] )
>>>
'''
对多个维度提供索引时，规则以此例来说明。
每个维度的索引数组，相对应的位置的值，组成一个坐标，在本例中。
组合之后的坐标即为:
(0, 2) (1, 1)
(1, 3) (2, 3)
'''
>>> a[i,j]                                     # i and j must have equal shape
array([[ 2,  5],
       [ 7, 11]])
>>>
>>> a[i,2]
array([[ 2,  6],
       [ 6, 10]])
>>>
>>> a[:,j]                                     # i.e., a[ : , j]
array([[[ 2,  1],
        [ 3,  3]],
       [[ 6,  5],
        [ 7,  7]],
       [[10,  9],
        [11, 11]]])
```

当索引列表包含重复时，数据分配会多次完成，留下最后一个值：

```python
>>> a = np.arange(5)
>>> a[[0,0,2]]=[1,2,3]
>>> a
array([2, 1, 3, 3, 4])
```

### 使用布尔数组进行索引

使用与原始数组具有 *相同形状的* 布尔数组：

```python
>>> a = np.arange(12).reshape(3,4)
>>> b = a > 4
>>> b                                          # b is a boolean with a's shape
array([[False, False, False, False],
       [False,  True,  True,  True],
       [ True,  True,  True,  True]])
>>> a[b]                                       # 1d array with the selected elements
array([ 5,  6,  7,  8,  9, 10, 11])
```

对于数组的**每个维度**，我们给出一个一维布尔数组，选择我们想要的切片：

> 1D布尔数组的长度需要与轴的长度一致54

```python
>>> a = np.arange(12).reshape(3,4)
>>> b1 = np.array([False,True,True])             # first dim selection
>>> b2 = np.array([True,False,True,False])       # second dim selection
>>>
>>> a[b1,:]                                   # selecting rows
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> a[b1]                                     # same thing
array([[ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> a[:,b2]                                   # selecting columns
array([[ 0,  2],
       [ 4,  6],
       [ 8, 10]])
>>>
>>> a[b1,b2]                                  # a weird thing to do
array([ 4, 10])
```

#### ix_(args)函数：

- 参数：1-D sequences应该为整数或布尔数组
- 返回一个ndarray元组，n个array返回的元组就是n维的

```python
>>> a = np.array([2,3,4,5])
>>> b = np.array([8,5,4])
>>> c = np.array([5,4,6,8,3])
>>> ax,bx,cx = np.ix_(a,b,c)
>>> ax
array([[[2]],
       [[3]],
       [[4]],
       [[5]]])
>>> bx
array([[[8],
        [5],
        [4]]])
>>> cx
array([[[5, 4, 6, 8, 3]]])
```

