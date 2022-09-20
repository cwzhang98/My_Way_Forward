# 1.数据类型

## 变量赋值

> python为弱类型语言，变量的赋值不需要声明

```python
count = 100
mlies = 1000.0 #浮点型
name = "zhang"

# 多变量赋值（用逗号隔开）
a, b, c = 1, ,2, 3
```

## 标准数据类型

- Numbers
- String
- List
- Tuple(元组) 类似于c++map容器
- Dictionary

 # 2.运算符

> 基本与其它语言一致，新增如下：

- ** 幂运算符

    ```python
    a, b = 2, 4
    print(a ** b) #a的b次方
    ```

- // 整除运算符(向下取整)

    ```python
    9 // 2
    # 结果为4
    ```

- 逻辑运算符：`and`,`not`,`or`

# 3.流程控制

## if语句

> pyhton中的else if变为elif，示例如下

```python
a = 10
if a < 2:
  print("zhang")
elif a < 2:
  print("c")
else:
  print("w")
  
```

## for，while语句

> 类似于js中的for in循环

```python
words = ['cat', 'window', 'lion']
for item in words:
  print(item, len(item)) #len()函数返回字符串的长度
  
  
# fibonacci
a = 0 
while a < 10:
  a, b = b, a + b
  
  
# for， while循环也有else子句，用法为：循环中，元素全部循环完毕时，或while条件为false时，执行else子句
for x in range(10):
  print(x)
else:
  print("循环完毕")
```

**循环中的 break、continue关键字的作用与其它语言一致**

>  range()函数,用于遍历数字序列,可添加参数，指定序列长度

```python
for i in range(5):
  print(i)
```

## pass语句

**`pass`语句不执行任何操作语法上需要一个语句，但程序不实际执行任何动作时，可以使用该语句。**

# 4.函数

## 定义函数

**定义函数使用关键字`def`，后跟函数名与括号内的形参列表。函数语句从下一行开始，并且必须缩进。**

> 函数内的第一条语句是字符串时，该字符串就是文档字符串，也称为 *docstring*

```python
def fib(n):
  a, b = 0, 1
  while a < n:
    print(a, end=' ')
    a, b = b, a+b
    print()
# Now call the function we just defined:
fib(2000)
```

### **python函数特性**

- `return` 语句不带表达式参数时，返回 `None`。函数执行完毕退出也返回 `None`。
- 可自定义参数默认值，与c++类似
- 函数默认值只计算一次，即解释器在函数定义时分配的默认值
- 在默认值为引用类型（列表，字典，对象）时，默认值会变化。与js规则类似

### 关键字参数

> 在自定义函数的参数默认值后，调用函数时，传入的实参只会按顺序赋值给形参。
>
> 关键字参数可以指定实参具体传入哪个形参

**语法：函数名(形参名=实参值)**

```python
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.")
    print("-- Lovely plumage, the", type)
    print("-- It's", state, "!")
    
parrot(voltage=1000000, action='VOOOOOM')             # 指定vlotage和action
parrot(action='VOOOOOM', voltage=1000000)             
```

**注意： 函数调用时，关键字参数必须跟在位置参数后面。**

### 多余参数

最后一个形参为 `**name` 形式时，接收一个字典，字典包含多余的(即函数定义中没有的)<u>***关键字***</u>参数

`*name`形参必须在`**name`形参之前，接受一个元组，元组包含多余的(即函数定义中没有的)位置参数

### 特殊参数

> 用`/`和`*`限制参数的传递方式

```python
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
  return 0

# /前面的参数为未知参数，/与*中间的参数为位置参数或关键字参数，*后面的参数为关键字参数
```

### 参数解包

调用函数时，用*从列表或元组中解包出来

```python
args = [3, 4, 5]
list(range(*args)) # 解包成3, 4, 5
```

### Lambda表达式

`lambda`关键字用于创建匿名函数，语法为：

lambda <paramaters>: <expresson>

```python
  def myfunc(n):
    return lambda x : x + n
  f = myfunc(5)
  f(0) # 5
  f(1) # 6
```

# 5.数据结构

## 列表

>  基本上就是普通数组，定义方法跟js类似

```python
pets = ['cat', 'dog', 'sheep']
```

### 列表方法

- append() 在列表末尾添加一个元素，相当于`a[len(a):] = [x]`

- extend(iterable)用可迭代对象的元素扩展列表，相当于`a[len(a):]=iteralbe` 

- Insert(i,x) 在指定位置插入元素，i为插入位置的索引

- remove(x) 从列表中删除第一个值为x的元素。若未找到元素，触发`VauleError`

- pop([i]) 删除列表中i位置的元素，并***返回***被删除的元素。未指定位置时，返回列表的最后一个元素。

- clear() 删除列表中所有的元素，相当于`del a[:]`

- Index(x, start, end) 返回列表中第一个值为x的元素的索引。start和end指定搜索范围。

- count(x) 返回列表中元素x出现的次数

- sort(cmp, key,  reverse)或sorted(iterable, amp, key, reverse). 

    - iterable为指定要排序的对象(sorted)

        ```python
        x  = [ 4 ,  6 ,  2 ,  1 ,  7 ,  9 ]
        y  =  sorted (x)
        print  y  #[1, 2, 4, 6, 7, 9]
        print  x  #[4, 6, 2, 1, 7, 9] 
        ```

        

    - cmp为函数，指定排序时进行比较的函数，可以指定一个函数或者lambda函数。

        ```python
        def  comp(x, y):
        if  x < y:
        return  1
        elif  x > y:
        return  - 1
        else :
        return  0
          
        nums  =  [ 3 ,  2 ,  8  , 0  ,  1 ]
        nums.sort(comp)
        print  nums  # 降序排序[8, 3, 2, 1, 0]
        nums.sort( cmp )  # 调用内建函数cmp ，升序排序
        print  nums  # 降序排序[0, 1, 2, 3, 8]
        
        ```

        

    - key为函数，指定取待排序元素的哪一项进行排序。

        ```python
        alist  =  [
          ( '2' ,  '3' ,  '10' ), 
          ( '1' ,  '2' ,  '3' ), 
          ( '5' ,  '6' ,  '7' ), 
          ( '2' ,  '5' ,  '10' ), 
          ( '2' ,  '4' ,  '10' )
        ]
        # 多级排序，先按照第3个元素排序，然后按照第2个元素排序：
        print  sorted (alist,  cmp  =  None , key  =  lambda  x:( int (x[ 2 ]),  int (x[ 1 ])), reverse  =  False )
        ```

    - reverse为布尔值，指定排序是否逆序

- reverse() 列表逆序
- copy() 返回列表的浅拷贝。相当于 a[:] 。

#### 用列表实现栈

```python
stack = [3, 4, 5]
stack.append(6)
stack.append(7)
 #[3, 4, 5, 6, 7]

stack.pop()
# 7
stack.pop()
# 6
stack.pop()
# 5
```

#### 用列表实现队列

使用[`collections.deque`](https://docs.python.org/zh-cn/3.9/library/collections.html#collections.deque)，可以快速从两端添加或删除元素。

```python
from collections import deque
queue = deque(["Eric", "John", "Michael"])
queue.append("Terry")           # Terry arrives
queue.append("Graham")          # Graham arrives
queue.popleft()                 # The first to arrive now leaves
#‘Eric'
queue.popleft()                 # The second to arrive now leaves
#'John'
```

#### 列表推导式

列表推导式的方括号内包含以下内容：一个表达式，后面为一个 `for` 子句，然后，是零个或多个 `for` 或 `if` 子句。结果是由表达式依据 `for` 和 `if` 子句求值计算而得出一个新列表。 举例来说，以下列表推导式将两个列表中不相等的元素组合起来：

```python
[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
#[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

# 等价于
for x in [1,2,3]:
     for y in [3,1,4]:
         if x != y:
             combs.append((x, y))
```

列表推导式可以使用复杂的表达式和嵌套函数：

```python
from math import pi
[str(round(pi, i)) for i in range(1, 6)]
#['3.1', '3.14', '3.142', '3.1416', '3.14159']
```

#### 嵌套的列表推导式

```python
 matrix = [
     [1, 2, 3, 4],
     [5, 6, 7, 8],
     [9, 10, 11, 12],
]
[[row[i] for row in matrix] for i in range(4)]
#[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

# 6. del语句

## del语句按索引从列表中移除元素

```python
a = [-1, 1, 66.25, 333, 333, 1234.5]
del a[0]
#[1, 66.25, 333, 333, 1234.5]
del a[2:4]
#[1, 66.25, 1234.5]
del a[:]
#[]
```

## del语句用来删除变量

```python
a = 10
del a
```

# 7.元组

## 初始化

- 元素之间用逗号隔开

    ```python
    t = 12345, 54321, 'hello!'
    # (12345, 54321, 'hello!')
    ```

- 更常用的是圆括号()括起来,方便表示嵌套关系

    ```python
    t = （12345, 54321, 'hello!'）
    
    a = () # 空元组
    ```

> 元组中的元素是不可改变的，一般可包含异质元素序列，通过解包（见本节下文）或索引访问

- 元组可以逆操作（类似于js解构赋值）

    ```python
    t = 12345, 54321, 'hello!'
    x, y, z = t #也称为序列解包
    ```

- 单个元素的元组

    ```python
    t = 'hello',
    #后面加个逗号
    ```

    

## 访问元组

通过下标索引访问，跟列表一样支持切片

```python
tup2 = (1, 2, 3, 4, 5, 6, 7 )
tup2[1:5]
```

## 修改元组

元组不允许修改，但是可以连接元组

```python
tup1 = (12, 34.56)
tup2 = ('abc', 'xyz')
tup3 = tup1 + tup2
```

## 删除元组

用del删除整个元组，元组中的元素不能删除

## 内置函数

- cmp(tuple1, tuple2) 比较两个元组元素。
- len(tuple) 计算元组元素个数。
- max(tuple) 返回元组中元素最大值。
- min(tuple) 返回元组中元素最小值。
- tuple(seq) 将列表转化为元组

# 8.集合

> 集合是由不重复元素组成的无序容器。基本用法包括成员检测、消除重复元素。集合对象支持合集、交集、差集、对称差分等数学运算。
>
> 创建集合用{...}或set()函数，不能用空的{}，{}创建的是字典

```python
basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
#{'orange', 'banana', 'pear', 'apple'}

'orange' in basket #成员检测
#True

"""
  比较运算符 in 和 not in 校验序列里是否存在某个值。
  运算符 is 和 is not 比较两个对象是否为同一个对象。
  所有比较运算符的优先级都一样，且低于数值运算符。
"""


a = set('abracadabra') #自动去重
#{'a', 'r', 'b', 'c', 'd'}

'''
支持集合运算 -,&,|,^

-求差集合， a-b求在a中但不在b中的元素
｜求全集，a｜b求在a中或在b中的元素
&求交集，a&b求ab交集
^求除去交集的全集，a^b相当于(a|b)-(a&b)
'''

```

集合也支持推导式

```python
a = {x for x in 'abracadabra' if x not in 'abc'}
#{'r', 'd'}
```

集合的内置方法详见：[python集合](https://www.runoob.com/python3/python3-set.html)

# 9.字典

> - 与以连续整数为索引的序列不同，字典以 *关键字* 为索引，关键字通常是字符串或数字，也可以是其他任意不可变类型。只包含字符串、数字、元组的元组，也可以用作关键字。
>
> - 字典的主要用途是通过关键字存储、提取值。用 `del` 可以删除键值对。用已存在的关键字存储值，与该关键字关联的旧值会被取代。通过不存在的键提取值，则会报错。
> - 对字典执行 `list(d)` 操作，返回该字典中所有键的列表,如需排序，请使用 `sorted(d)`
> - keys()方法，返回字典键值列表的可迭代对象

[`dict()`](https://docs.python.org/zh-cn/3.9/library/stdtypes.html#dict) 构造函数可以直接用键值对序列创建字典：

```python
dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
#{'sape': 4139, 'guido': 4127, 'jack': 4098}
```

字典推导式可以用任意键值表达式创建字典：

```python
{x: x**2 for x in (2, 4, 6)}
#{2: 4, 4: 16, 6: 36}
```

用关键字参数创建字典：

```python
dict(sape=4139, guido=4127, jack=4098)
#{'sape': 4139, 'guido': 4127, 'jack': 4098}
```

## 循环的技巧

- 在字典中循环时，用 `items()` 方法可同时取出键和对应的值：

    ```python
    knights = {'gallahad': 'the pure', 'robin': 'the brave'}
    for k, v in knights.items():
       print(k, v)
    ```

    

- 在序列中循环时，用 [`enumerate()`](https://docs.python.org/zh-cn/3.9/library/functions.html#enumerate) 函数可以同时取出位置索引和对应的值：

```python
for i, v in enumerate(['tic', 'tac', 'toe']):
    print(i, v)
```

- 同时循环两个或多个序列时，用 [`zip()`](https://docs.python.org/zh-cn/3.9/library/functions.html#zip) 函数可以将其内的元素一一匹配：

    ```python
    # zip函数返回一个对象,list()函数可将此对象转化为元组列表
    # zip(*zipedobj)为解压 
      
    questions = ['name', 'quest', 'favorite color']
    answers = ['lancelot', 'the holy grail', 'blue']
    for q, a in zip(questions, answers):
      print('What is your {0}?  It is {1}.'.format(q, a))
    ```

- 使用 [`set()`](https://docs.python.org/zh-cn/3.9/library/stdtypes.html#set) 去除序列中的重复元素。

- ```python
    basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
    for f in sorted(set(basket)):
      print(f)
    ```

    # 10.模块

    

> Python 模块(Module)，是一个 Python 文件，以 .py 结尾，包含了 Python 对象定义和Python语句。

## import语句

模块定义好后，可在另外的文件用用 `import <模块名>`的方式引入模块。

用`<模块名>.<函数名>`或`<模块名>.<变量名>`的方式使用模块中的函数和变量。

## from…import 语句

Python 的 from 语句让你从模块中导入一个指定的部分到当前命名空间中。语法如下：

```python
from modname import name1[, name2[, ... nameN]]
```

例如，要导入模块 fib 的 fibonacci 函数，使用如下语句：

```python
from fib import fibonacci
```

## from…import* 语句

把一个模块的所有内容全都导入到当前的命名空间也是可行的，只需使用如下声明：

```python
from modname import *
```

## `__name__`属性

每个模块都有一个__name__属性，当其值是'__main__'时，表明该模块自身在运行，否则是被引入。

```python
if __name__ == '__main__':
   print('程序自身在运行')
else:
   print('我来自另一模块')
```



## python标准模块库

参考[python标准模块库](https://docs.python.org/zh-cn/3.9/library/index.html)

## dir()函数

内置函数 [`dir()`](https://docs.python.org/zh-cn/3.9/library/functions.html#dir) 用于查找模块定义的名称。返回结果是经过排序的字符串列表：

```python
import fibo, sys
dir(fibo)
#['__name__', 'fib', 'fib2']
```

没有参数时，[`dir()`](https://docs.python.org/zh-cn/3.9/library/functions.html#dir) 列出当前定义的名称：

```python
a = [1, 2, 3, 4, 5]
import fibo
fib = fibo.fib
# ['__builtins__', '__name__', 'a', 'fib', 'fibo', 'sys']
```

# 10.输入与输出

## 格式化字符串字面值

（简称为 f-字符串）在字符串前加前缀 `f` 或 `F`，通过 `{expression}` 表达式，把 Python 表达式的值添加到字符串内。

```python
import math
print(f'The value of pi is approximately {math.pi:.3f}.')
#The value of pi is approximately 3.142.
```

在 `':'` 后传递整数，为该字段设置最小字符宽度，常用于列对齐：

```python
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
for name, phone in table.items():
  print(f'{name:10} ==> {phone:10d}')
"""
Sjoerd     ==>       4127
Jack       ==>       4098
Dcab       ==>       7678
"""
```

## 字符串 format() 方法

基本用法如下：

```python
print('We are the {} who say "{}!"'.format('knights', 'Ni'))
#We are the knights who say "Ni!"
```

花括号及之内的字符（称为格式字段）被替换为传递给 [`str.format()`](https://docs.python.org/zh-cn/3.9/library/stdtypes.html#str.format) 方法的对象。花括号中的数字表示传递给 [`str.format()`](https://docs.python.org/zh-cn/3.9/library/stdtypes.html#str.format)方法的对象所在的位置。

```python
print('{0} and {1}'.format('spam', 'eggs'))
# spam and eggs
print('{1} and {0}'.format('spam', 'eggs'))
# eggs and spam
```

使用关键字参数：

```python
print('This {food} is {adjective}.'.format(
  food='spam', adjective='absolutely horrible'))
#This spam is absolutely horrible.
```

可以用 '**' 符号，把 table 当作传递的关键字参数。

```python
table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab: {Dcab:d}'.format(**table))
```

> 补充：*和**符号出了算术运算之外，还有其它重要作用
>
> *用于列表和元组，用于解包
>
> **用于字典，将字典解开成关键字参数



## 格式化字符串的方法

详见文档

## 读写文件

`open(filename, mode, encoding=None)`

```
f = open('workfile', 'w', encoding="utf-8")
```

- 第一个实参是文件名字符串。
- 第二个实参是包含描述文件使用方式字符的字符串。
    - *mode* 的值包括 `'r'` ，表示文件只能读取；
    - `'w'`表示只能写入（现有同名文件会被覆盖）；
    - `'a'` 表示打开文件并追加内容，任何写入的数据会自动添加到文件末尾。
    - `'r+'` 表示打开文件进行读写。*mode* 实参是可选的，省略时的默认值为 `'r'`。

- 第三个参数为编码规则

### 使用`with`关键字

优点是，子句体结束后，文件会正确关闭，即便触发异常也可以。

```python
with open('workfile', encoding="utf-8") as f:
  read_data = f.read()
f.closed #True
```

> 调用 `f.write()` 时，未使用 `with` 关键字，或未调用 `f.close()`，即使程序正常退出，也**可能** 导致 `f.write()`的参数没有完全写入磁盘。

## 文件对象的方法

- `f.read(size)` 可用于读取文件内容，它会读取一些数据，并返回字符串（文本模式），或字节串对象（在二进制模式下）。 *size*是可选的数值参数。

- `f.readline()` 从文件中读取单行数据；字符串末尾保留换行符（`\n`），只有在文件不以换行符结尾时，文件的最后一行才会省略换行符。

- 从文件中读取多行时，可以用循环遍历整个文件对象。

    ```python
    for line in f:
       print(line, end='')
    ```

- `f.write(string)` 把 *string* 的内容写入文件，并返回写入的字符数。

- `f.tell()` 返回整数，给出文件对象在文件中的当前位置，表示为二进制模式下时从文件开始的字节数，以及文本模式下的意义不明的数字。

 ## 使用json保存结构化数据

[`json`](https://docs.python.org/zh-cn/3.9/library/json.html#module-json) 标准模块采用 Python 数据层次结构，并将之转换为字符串表示形式；这个过程称为 *serializing* （序列化）。从字符串表示中重建数据称为 *deserializing* （解序化）。

- 只需一行简单的代码即可查看某个对象的 JSON 字符串表现形式：

```python
import json
x = [1, 'simple', 'list']
json.dumps(x)
# '[1, "simple", "list"]'
```

- Dumps()的变体dump()，将对象序列化为 [text file](https://docs.python.org/zh-cn/3.9/glossary.html#term-text-file) 。因此，如果 `f` 是 [text file](https://docs.python.org/zh-cn/3.9/glossary.html#term-text-file) 对象，可以这样做：

```python
json.dump(x, f)
# 将json字符串输入文件f中
x = json.load(f)
# 读取json数据
```

# 11.类

## 类定义

基础语法：

```python
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

## 类对象

类对象支持两种操作：属性引用和实例化。

### 属性引用 

使用 Python 中所有属性引用所使用的标准语法: `obj.name`。 有效的属性名称是类对象被创建时存在于类命名空间中的所有名称。

```
class MyClass:
    """A simple example class"""
    i = 12345

    def f(self):
        return 'hello world'
```

那么 `MyClass.i` 和 `MyClass.f` 就是有效的属性引用，将分别返回一个整数和一个函数对象。 类属性也可以被赋值，因此可以通过赋值来更改 `MyClass.i` 的值。

### 实例化

类的 *实例化* 使用函数表示法。 可以把类对象视为是返回该类的一个新实例的不带参数的函数。 举例来说（假设使用上述的类）:

```python
x = MyClass()
```

### 构造函数

类有一个名为 `__init__()` 的特殊方法（**构造方法**），该方法在类实例化时会自动调用，像下面这样：

```python
def __init__(self):
    self.data = []
```

类定义了 __init__() 方法，类的实例化操作会自动调用 __init__() 方法。

>self代表类的实例，而非类对象

> 类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的**第一个参数名称**, 按照惯例它的名称是 self。

```python
class Test:
    def prt(self):
        print(self)
        print(self.__class__)
 
t = Test()
t.prt()
```

## 继承

派生类的定义如下：

```python
class DerivedClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```

子类会继承父类的的属性和方法，BaseClassName（实例中的基类名）必须与派生类定义在一个作用域内。

```python
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
 #单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
 
 
 s = student('ken',10,60,3)
s.speak()
```

## 多继承

多继承的类定义如下例：

```python
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

需要注意圆括号中父类的顺序，若是父类中有相同的方法名，而在子类使用时未指定，python从左至右搜索 即方法在子类中未找到时，从左到右查找父类中是否包含方法。

```python
#类定义
class people:
    #定义基本属性
    name = ''
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
        self.__weight = w
    def speak(self):
        print("%s 说: 我 %d 岁。" %(self.name,self.age))
 
 #单继承示例
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构函
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
 
 #另一个类，多重继承之前的准备
class speaker():
    topic = ''
    name = ''
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s"%(self.name,self.topic))
 
 #多重继承
class sample(speaker,student):
    a =''
    def __init__(self,n,a,w,g,t):
        student.__init__(self,n,a,w,g)
        speaker.__init__(self,n,t)
 
 test = sample("Tim",25,80,4,"Python")
test.speak()   #方法名同，默认调用的是在括号中参数位置排前父类的方法
```

## 方法重写

如果你的父类方法的功能不能满足你的需求，你可以在子类重写你父类的方法，实例如下：

```python
class Parent:        # 定义父类
   def myMethod(self):
      print ('调用父类方法')
 
class Child(Parent): # 定义子类
   def myMethod(self):
      print ('调用子类方法')
 
c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法
super(Child,c).myMethod() #用子类对象调用父类已被覆盖的方法

"""
super函数可以调用继承链中任意父类的被覆盖的方法，语法为：
super(class, self)
"""
```

## 类属性与方法

### 类的私有属性和方法

两个下划线开头，声明该属性或方法为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 `self.__private_attrs`或`self.__private_method`。

### 类的方法

在类的内部，使用 def 关键字来定义一个方法，与一般函数定义不同，类方法必须包含参数 **self**，且为第一个参数，**self** 代表的是类的实例。

### 类的专有方法（魔法函数）

- **`__init__` :** 构造函数，在生成对象时调用
- **`__del__`:** 析构函数，释放对象时使用
- **`__repr__` :** 打印，转换
- **`__setitem__` :** 按照索引赋值
- **`__getitem__`:** 按照索引获取值
- **`__len__`:** 获得长度
- **`__cmp__`:** 比较运算
- **`__call__`:** 函数调用
- **`__add__`:** 加运算
- **`__sub__`:** 减运算
- **`__mul__`:** 乘运算
- **`__truediv__`:** 除运算
- **`__mod__`:** 求余运算
- **`__pow__`:** 乘方

## 运算符重载

用上述专有方法进行运算符重载

## 迭代器

容器对象：列表、元组、字典等。[`for`](https://docs.python.org/zh-cn/3.9/reference/compound_stmts.html#for) 语句会在容器对象上调用 [`iter()`](https://docs.python.org/zh-cn/3.9/library/functions.html#iter)。 该函数返回一个定义了 [`__next__()`](https://docs.python.org/zh-cn/3.9/library/stdtypes.html#iterator.__next__) 方法的迭代器对象，此方法将逐一访问容器中的元素。当元素用尽时，[`__next__()`](https://docs.python.org/zh-cn/3.9/library/stdtypes.html#iterator.__next__) 将引发 [`StopIteration`](https://docs.python.org/zh-cn/3.9/library/exceptions.html#StopIteration) 异常来通知终止 `for` 循环。 你可以使用 [`next()`](https://docs.python.org/zh-cn/3.9/library/functions.html#next) 内置函数来调用 [`__next__()`](https://docs.python.org/zh-cn/3.9/library/stdtypes.html#iterator.__next__) 方法；这个例子显示了它的运作方式:

```python
s = 'abc'
it = iter(s)
#<str_iterator object at 0x10c90e650>
next(it)
# 'a'
next(it)
# 'b'
next(it)
# 'c'
```

可以给自己类添加迭代器，定义一个`__iter__()`方法来返回一个带有`__next__()`方法的对象。

```python
class Reverse:
    """Iterator for looping over a sequence backwards."""
    def __init__(self, data):
        self.data = data
        self.index = len(data)

    def __iter__(self):
        return self

    def __next__(self):
        if self.index == 0:
            raise StopIteration  # 异常用于标识迭代的完成，防止出现无限循环的情况
        self.index = self.index - 1
        return self.data[self.index]
        
rev = Reverse('spam')
iter(rev) #生成迭代器

for char in rev:
    print(char)
```

## 生成器

在 Python 中，使用了 yield 的函数被称为生成器（generator）。跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

> 在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

```python
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a # yield会返回a的值，每次调用__next__()时,会使用保存的信息继续运行
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (__next__(f), end=" ")
    except StopIteration:
        sys.exit()
```

