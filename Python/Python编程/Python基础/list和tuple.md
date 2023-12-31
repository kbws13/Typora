# List(列表)

Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。

比如，列出班里所有同学的名字，就可以用一个list表示：

```python
classmates = ['Michael', 'Bob', 'Tracy']
print(classmates)
['Michael', 'Bob', 'Tracy']
```

变量`classmates`就是一个list。

## 获取list元素个数

用`len()`函数可以获得list元素的个数：

```python
len(classmates)
3
```

## 访问list元素

用索引来访问list中每一个位置的元素，记得索引是从`0`开始的：

```python
classmates[0]
'Michael'
classmates[1]
'Bob'
classmates[2]
'Tracy'
classmates[3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

当索引超出了范围时，Python会报一个`IndexError`错误，所以，要确保索引不要越界，记得最后一个元素的索引是`len(classmates) - 1`。

如果要取最后一个元素，除了计算索引位置外，还可以用`-1`做索引，直接获取最后一个元素：

```python
classmates[-1]
'Tracy'
```

以此类推，可以获取倒数第2个、倒数第3个：

```python
classmates[-2]
'Bob'
classmates[-3]
'Michael'
classmates[-4]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

当然，倒数第4个就越界了。

## 添加元素到末尾

list是一个可变的有序表，所以，可以往list中追加元素到末尾：

```python
classmates.append('Adam')
classmates
['Michael', 'Bob', 'Tracy', 'Adam']
```



## 插入指定指定位置

也可以把元素插入到指定的位置，比如索引号为`1`的位置：

```python
classmates.insert(1, 'Jack')
classmates
['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
```

## 删除末尾元素

要删除list末尾的元素，用`pop()`方法：

```python
classmates.pop()
'Adam'
classmates
['Michael', 'Jack', 'Bob', 'Tracy']
```

## 删除指定位置元素

要删除指定位置的元素，用`pop(i)`方法，其中`i`是索引位置：

```python
classmates.pop(1)
'Jack'
classmates
['Michael', 'Bob', 'Tracy']
```

## 替换某个元素

要把某个元素替换成别的元素，可以直接赋值给对应的索引位置：

```python
classmates[1] = 'Sarah'
classmates
['Michael', 'Sarah', 'Tracy']
```

## 包含的元素可以是不同类型

list里面的元素的数据类型也可以不同，比如：

```python
L = ['Apple', 123, True]
```

list元素也可以是另一个list，比如：

```python
s = ['python', 'java', ['asp', 'php'], 'scheme']
len(s)
4
```

要注意`s`只有4个元素，其中`s[2]`又是一个list，如果拆开写就更容易理解了：

```python
p = ['asp', 'php']
s = ['python', 'java', p, 'scheme']
```

要拿到`'php'`可以写`p[1]`或者`s[2][1]`，因此`s`可以看成是一个二维数组，类似的还有三维、四维……数组，不过很少用到。

如果一个list中一个元素也没有，就是一个空的list，它的长度为0：

```python
L = []
len(L)
0
```

# Tuple(元组)

另一种有序列表叫元组：tuple。tuple和list非常类似，但是==tuple一旦初始化就不能修改==，比如同样是列出同学的名字：

```python
classmates = ('Michael', 'Bob', 'Tracy')
```

现在，classmates这个tuple不能变了，它也没有append()，insert()这样的方法。其他获取元素的方法和list是一样的，你可以正常地使用`classmates[0]`，`classmates[-1]`，但不能赋值成另外的元素。

不可变的tuple有什么意义？因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。

## tuple的陷阱

当你定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来，比如：

```python
t = (1, 2)
print(t)
(1, 2)
```

如果要定义一个空的tuple，可以写成`()`：

```python
t = ()
print(t)
()
```

但是，要定义一个只有1个元素的tuple，如果你这么定义：

```python
t = (1)
print(t)
1
```

定义的不是tuple，是`1`这个数！这是因为==括号`()`既可以表示tuple，又可以表示数学公式中的小括号==，这就产生了歧义，因此，Python规定，这种情况下，按小括号进行计算，计算结果自然是`1`。

所以，只有1个元素的tuple定义时必须加一个逗号`,`，来消除歧义：

```python
t = (1,)
print(t)
(1,)
```

Python在显示只有1个元素的tuple时，也会加一个逗号`,`，以免你误解成数学计算意义上的括号。

来看一个“可变的”tuple：

```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向`'a'`，就不能改成指向`'b'`，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

理解了“指向不变”后，要创建一个内容也不变的tuple怎么做？那就必须保证tuple的每一个元素本身也不能变。