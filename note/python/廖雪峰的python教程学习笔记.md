# 高级特性

## 切片

```
L = ['Jack', 'Tom', 'Bob', 'Richard', 'Leo']
print L[0:3]
print L[:3]

['Jack', 'Tom', 'Bob']
['Jack', 'Tom', 'Bob']

```
`L[0:3]`从索引0开始获取元素，直到索引3为止，但不包括索引3，即索引：0，1，2，前三个元素。如果第1个索引为0也可省略`L[:3]`。

```
print L[-2:]

['Richard', 'Leo']

```
`L[-2:]`获取倒两个元素的值。

```
L = range(100)
print L[::5]

[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]
```
`L[::5]`每5个获取一个元素。

```
print L[:]

[0, 1, 2, 3, 4, ..., 97, 98, 99]
```
`L[:]`按原来的list复制一个新的list。

`tuple`元组和字符串也支持切片的操作。

## 迭代

python中通过`for ... in ...`来完成迭代，迭代可用在`list`、`tuple`和其它可迭代的对象上。

判断一个对象是否是可迭代对象，使用`collections`模块的`Iterable`类型判断：

```
from collections import Iterable
print isinstance('abc', Iterable)
print isinstance([1, 2, 3], Iterable)
print isinstance(123, Iterable)

True
True
False
```

`dict`字典类型的对象默认迭代时，迭代的是key：

```
d = {'a': 1, 'b': 2, 'c': 3}
for key in d:
    print key

a
c
b
```
如果要迭代value，可使用`for val in d.itervalues()`。同时迭代key和value可用`for key, val in d.iteritems()`。

如果要实现`list`迭代时迭代出元素的下标（索引），可使用内置的`enumerate`函数，`for i, val in enumerate(['a', 'b', 'c'])`。

## 列表生成式

> 列表生成式即List Comprehensions，是Python内置的非常简单却强大的可以用来创建list的生成式。

举个例子，要生成list`[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`可以用`range(1, 11)`：

```
print range(1, 11)

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
但如果要生成`[1x1, 2x2, 3x3, ..., 10x10]`怎么做？方法一是循环：

```
L = []
for x in range(1, 11):
    L.append(x * x)
print L

[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
使用循环相对比较繁琐，而使用列表生成式一行代码即可搞定：

```
print [x * x for x in range(1, 11)]

[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```
写列表生成式时，把要生成的元素`x * x`放到前面，后面跟`for`循环，就可以把list创建出来。

带`if`条件判断的列表生成式：

```
print [x * x for x in range(1, 11) if x % 2 == 0]

[4, 16, 36, 64, 100]
```

多层循环嵌套的列表生成式：

```
print [x + y for x in 'ABC' for y in 'XYZ']

['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']
```

**使用内置的`isinstance`函数判断一个变量是不是指定的数据类型**：

```
print isinstance('abc', str)
print isinstance(123, int)
print isinstance(True, bool)
print isinstance('False', bool)

True
True
True
False
```

## 生成器

> 按照某种算法推算，一边循环一边计算的机制，称为生成器。

### 创建生成器的方法：

第一种：把列表生成式的`[]`改为`()`即可：

```
L = [x for x in range(10)]
print L
g = (x for x in range(10))
print g

[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
<generator object <genexpr> at 0x0000000001E588B8>
```
*变量`L`和`g`的区别仅仅是最外层的`[]`和`()`，`L`是list，`g`是generator*

获取生成器的每一个元素，通过调用generator的`next()`方法：

```
print g.next()
print g.next()

0
1
```

# 函数式编程

> 函数式编程的一个特点就是，允许把函数本身作为一个参数传入另一个函数，还允许返回一个函数！

## 高阶函数

> 能够接收函数作为参数的函数，称为高阶函数（High-order Function）。

## 匿名函数

用关键字`lambda`表示：`lambda: x: x * x`。冒号前的`x`表示函数参数，冒号后的`x * x`为表达式。

**注：**匿名函数有一个限制，就是只能有一个表达式，不用写`return`，返回值就是该表达式的结果。
