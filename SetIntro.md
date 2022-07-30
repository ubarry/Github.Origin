---
title: "Intro to 'Set'"
format: html
author: Barry
institude: Clayton University
footer: "**上述代码全部来自本人在 miniconda 测试时的记录，版权所有，翻版不究**"
format: 
    revealjs:
        incremental: true
        self-contained: true
        theme: moon
---
## 1. 什么是集合（Set）?

我们知道，在PYTHON中一共有四种集合数据结构(Data Structure)类型，即列表(List)、元组(Tuple)、集合(Set)、和字典(Dictionary). 在四种数据类型里面，集合(Set)的性质和数学中的集合的概念最为接近。

所以，在PYTHON中，到底什么是集合(Set)呢? 如果你对PYTHON已经有了一定的了解，那么可以在帮助文件里找到PYTHON对集合(Set)的说明：

```PYTHON
>>> help(set)
Help on class set in module builtins:

class set(object)
 |  set() -> new empty set object
 |  set(iterable) -> new set object
 |
 |  Build an unordered collection of unique elements.
 |
 |  Methods defined here:
 |
 |  __and__(self, value, /)
 |      Return self&value.
 |
 |  __contains__(...)
 |      x.__contains__(y) <==> y in x.
 |
 |  __eq__(self, value, /)
 |      Return self==value.
 |
 |  __ge__(self, value, /)
 |      Return self>=value.
 |
 |  __getattribute__(self, name, /)
 |      Return getattr(self, name).
 |
 |  __gt__(self, value, /)
 |      Return self>value.
 |
 |  __iand__(self, value, /)
-- More  --
```

可以看到，一般我们对集合(Set)的定义是：无序且不重复的元素的集合(an unordered collection of unique elements)。

值得注意的是，PYTHON中的集合(Set)具有以下三个特征：

- 无序性

    无序性是指，集合(Set)内部存储的数据的顺序和一开始输入数据的顺序不一致，数据读取时的顺序和存入时的顺序也是不一致的。同时，集合(Set)中的元素不存在键或是下标，无法通过此类方法来查找某一元素。
    
    另外，无序性并不代表集合(Set)内部的存储顺序是随机的。集合(Set)对象建立之后，元素的顺序是相对固定的。具体的排序规则与哈希值有关，这里不作详细的介绍。

    在本文的第二部分中，你可以找到能验证集合(Set)这一特性的例子。

- 确定性

    给定一个集合(Set)，任给一个元素，该元素只能属于或者不属于该集合(Set)。这一性质与数学中的集合确定性的定义是一致的。

    从另一个方面来说，集合(Set)存在一种方法，可以判定给定的元素是否属于该集合(Set)。
    
    在小节 3-4 中，会稍微涉及一些这方面的内容。

- 不重复性

    不重复性意味着集合(Set)不允许包含重复的成员。
    
    不重复性需要保证添加的元素按照 equals() 判断时，不能返回 true ，只能返回 false ，相同的元素只能存储一个。如果添加了集合(Set)中已有的成员，那么这一添加不会带来任何改变。

    在第二部分和第三部分的第 1 小节中，你可以找到关于不重复性的例子。

## 2. 关于如何建立一个集合(Set)对象

- 只要是可哈希的对象，都能成为集合的元素。

### 2-1 直接创建
我们可以用大括号{}来创建一个集合(Set)，元素间用逗号来分割。

请看下面的例子：

```PYTHON
#创建一个名为 x1 的集合，并输出集合的内容：
>>> x1={"hen","rabbit","turtle"}
>>> print(x1)
{'turtle', 'hen', 'rabbit'}
#重复出现的元素在创建时会自动合并：
>>> myset={1,1,4,5,1,4,1,9,1,9,8,1,0,'(悲)'}
>>> print(myset)
{0, 1, 4, 5, 8, 9, '(悲)'}
#直接使用空的大括号不能如愿地创建一个空集合，解释器会将它视为一个空字典
>>> yourset={}
>>> print(yourset)
{}
>>> yourset.add(1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'dict' object has no attribute 'add'
>>>
```

### 2-2 使用 set() 函数
set() 函数能将列表、元组等其他类型的数据转换为集合(Set)，如果原来的数据中存在重复元素，则在转换为集合(Set)时会将冗余的部分删除。

- 参数要求：

    1. 必须是可迭代的类型，比如字符串，列表，元组，迭代器等。输入整数等不可迭代数据会报错。 

    2. 将构成集合的元素的数据必须是不可变的类型。

    3. set() 的参数只能有一个

- set() 实质：内部进行可迭代性的 for 循环

使用例：

```PYTHON
#将字符串转为集合
>>> x=set('EncyclopediaBritannica')
>>> print(x)
{'a', 'i', 'B', 'E', 'l', 't', 'r', 'c', 'n', 'd', 'e', 'y', 'o', 'p'}
#将列表转为集合
>>> y=set(['a', 'i', 'B', 'E', 'l', 't', 'r', 'c', 'n', 'd', 'e', 'y', 'o', 'p'])
>>> print(y)
{'i', 'B', 'E', 'l', 't', 'c', 'n', 'o', 'a', 'd', 'e', 'r', 'y', 'p'}
#将元组转为集合
>>> fisher=set((1,2,3))
>>> print(fisher)
{1, 2, 3}
#调用不带参的 set() 函数的情况：创建一个空集合
>>> z=set()
>>> print(z)
set()
>>> z.add(1)
>>> print(z)
{1}
>>>
```

### 2-3 注意事项
我们需要明确的是，一般而言，集合(Set)本身是不可哈希，即可以改变的。但是集合的元素必须是可哈希，即不可变的。如果为集合(Set)添加不可哈希的元素，则会报错。

```PYTHON
>>> x=1
>>> s0={1,2,3}
>>> s1={3.14159265358979323846,2.71828,299792458}
#在PYTHON中，只要变量的值是可哈希的，变量就能用来为集合添加元素
>>> s1.add(x)
>>> print(s1)
{1, 2.71828, 3.141592653589793, 299792458}
#上面添加进集合 s1 的是变量 x 的值，在外部赋给变量 x 其他的值并不能改变 s1 中对应的元素
>>> x=99
>>> print(s1)
{1, 2.71828, 3.141592653589793, 299792458}
#集合是不可哈希的，因而集合不能成为另一个集合的元素
>>> s1.add(s0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'set'
>>> s1={3.14159265358979323846,2.71828,299792458,1,s0}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'set'
>>>
```

当然，也存在特殊的不可变集合。例如 frozenset 。它执行速度快，可以作为dict字典的Key，也可以成为其他集合的元素。

```PYTHON
>>> y=frozenset([1,2,3])
>>> print(y)
frozenset({1, 2, 3})
# frozenset() 不允许改变内容
>>> y.add(4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'frozenset' object has no attribute 'add'
# frozenset() 可以成为另一集合的元素
>>> y1={1,2,3,y}
>>> print(y1)
{frozenset({1, 2, 3}), 1, 2, 3}
>>>
```

## 3. 对集合(Set)可以进行那些操作？

以下所包含的函数是 Set 的方法（类函数），在调用某个 Set 对象名下的函数时，需要加上相应的前缀。

例如，对于 2-1 中即将讲到的函数 add() ，可以这样调用：

```PYTHON
>>> x1={1,2,3}
>>> x1.add("Jill")
>>> print(x1)
{1, 2, 3, 'Jill'}
```

当然，细心的读者可能已经注意到了，关于这一部分的内容，PYTHON在集合的帮助文件里已经给出了比较全面的概括。虽然在第一部分由于篇幅原因并未全部展示，不过有兴趣的话可以在PYTHON的终端键入 help(set) 自行了解。这里我们仅介绍一些比较常用的方法。

### 3-1 增
- add()

  为集合(Set)添加一个指定的新元素。关于 add() 的使用方法，前文已经出现了非常多的例子，这里就不多作赘述。

  另外，利用集合(Set)自身的特性，add() 方法可以用于判断某个对象是否是可哈希的。它也可以被用于数据的去重操作。

- update()

  如果要同时为集合(Set)添加多个指定的元素，应考虑使用 update() 方法。

  调用 update() 时，会将可迭代对象拆分成元素并入原集合。当然如果遇到重复的元素，会自动去重。

  我们可以这样描述 update() 函数：
  set_1.update(set_2,list_1,tuple_1,dict_1,str_1, ... )

  值得注意的是：
  - update()方法的参数可以是一个或多个诸如集合(Set)，列表(List)，元组(Tuple)，字典(Dictionary)，字符串(String)的可迭代对象，不能是整数或浮点数等不可迭代的对象。
  - update()方法没有返回值，仅改变它的前缀对应的集合(Set)。
  - 将字典作为 update() 的参数时，将键作为元素分别加入原集合。字典的值则不参与“更新”。

  示例：
  
  ```PYTHON
  #参数类型为集合
  >>> touhou={'marisa','reimu','patchouli','reimilia','flandre','sakuya'}
  >>> temp={"great fairy","koakuma","meiling","cirno"}
  >>> touhou.update(temp)
  >>> print(touhou)
  {'patchouli', 'koakuma', 'flandre', 'reimu', 'cirno', 'reimilia', 'sakuya', 'marisa', 'great fairy', 'meiling'}
  #参数类型为列表
  >>>  temp=[9]
  >>> touhou.update(temp)
  >>> print(touhou)
  {9, 'patchouli', 'koakuma', 'flandre', 'reimu', 'cirno', 'reimilia', 'sakuya', 'marisa', 'great fairy', 'meiling'}
  #参数类型为字典，效果是将键作为新成员加入原集合
  >>> temp=       {
  ...     'satori':'6102_e75b014a75db409dbbd3eb586bf2e0cd.jpeg',
  ...     'aya':'6102_7536de948c524eafbae4fc7010262800.jpeg',
  ...     'youmu':'youmu.jpeg',
  ... }
  >>> touhou.update(temp)
  >>> print(touhou)
  {'satori', 'aya', 9, 'patchouli', 'youmu', 'koakuma', 'flandre', 'reimu', 'cirno', 'reimilia', 'sakuya', 'marisa', 'great fairy', 'meiling'}
  #参数类型为字符串
  >>> shatter=set()
  >>> shatter.update('Polymerase Chain Reaction')
  >>> print(shatter)
  {'n', 's', 'y', 't', 'R', 'P', 'h', 'e', 'c', 'r', 'l', 'm', 'C', 'i', ' ', 'a', 'o'}
  >>>
  ```

- copy()

  这是一个无参的函数。可以实现集合(Set)的克隆。

  ```PYTHON
  >>> onetoten={1,2,3,4,5,6,7,8,9,10}
  >>> positive_integer_below_11=onetoten.copy()
  >>> print(positive_integer_below_11)
  {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
  ```

### 3-2 删
- pop()

    删除当前最后一位的元素(?)，返回值是删除的那个元素。
    
    注意，由于集合(Set)内部的元素是无序的, 调用 pop() 时并不会按照输入的顺序删除元素;  help(set) 的描述中说删除是“随机(arbitrary)”的，但是实际上删除的顺序似乎依然遵循着某种规律。

- remove()
  
  要指定一个元素将其从集合(Set)中删除，可以使用 remove() 方法。

  需要注意的是，这个值必须在集合(Set)中存在，否则会引发 KeyERROR 错误。

- discard()
  
  指定一个元素将其从集合(Set)中删除。

  与 remove() 不同的是，参数里给出的对象不需要存在于集合中。即使在集合(Set)中找不到指定的对象，也不会触发报错。

- clear()
  
  使用 clear() 方法，可以清空集合(Set)，将其变为一个空集。

- del
  
  使用 del 方法，可以彻底删除这个集合。操作完成后，该集合对象将会消失。

  注意 del 不是函数，书写时需要注意语句表达方式。

3-2 方法使用例：

```PYTHON
>>> soda=set()
>>> soda.add('Cyber Punk: 2077')
>>> soda.update("Bartender Action",{'Jill','VA-11 Hall-A'})
>>> soda.update({1,2,3,4,5,16891,'pi',3.14})
>>> print(soda)
{1, 2, 3, 4, 5, 3.14, 'd', 'B', 'Cyber Punk: 2077', 'A', 'VA-11 Hall-A', 'pi', 'r', 'i', 'Jill', 'n', ' ', 'a', 't', 'e', 16891, 'c', 'o'}
#使用 pop() 冒泡删除集合 soda 的 10 个元素
>>> for i in range(10):
...     soda.pop()
...
1
2
3
4
5
3.14
'd'
'B'
'Cyber Punk: 2077'
'A'
>>> print(soda)
{'VA-11 Hall-A', 'pi', 'r', 'i', 'Jill', 'n', ' ', 'a', 't', 'e', 16891, 'c', 'o'}
#使用 remove() 方法删除元素。这里终端重启过，下面的集合 soda 是重新另建的，因而打印的返回结果中，元素顺序发生了变化
>>> soda.remove('pi')
>>> print(soda)
{' ', 't', 'e', 'VA-11 Hall-A', 'a', 'r', 'c', 'i', 'Jill', 16891, 'n', 'o'}
>>> soda.remove(' ')
#给 remove() 提供不属于该集合的参数会导致报错
>>> soda.remove('16891')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: '16891'
#给 discard() 提供不属于该集合的参数不会导致报错，运行后也不会改变原集合的任何内容
>>> soda.discard('16891')
>>> print(soda)
{'t', 'e', 'VA-11 Hall-A', 'a', 'r', 'c', 'i', 'Jill', 16891, 'n', 'o'}
>>> soda.discard('n')
>>> print(soda)
{'t', 'e', 'VA-11 Hall-A', 'a', 'r', 'c', 'i', 'Jill', 16891, 'o'}
#使用 clear() 函数会将目标集合变为空集
>>> soda.clear()
>>> print(soda)
set()
#使用 del 方法可以删除集合对象本身
>>> soda={'spirit','Coco-Cola','Pepsi-Cola'}
>>> del soda
>>> print(soda)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'soda' is not defined
>>>
```

### 3-3 获取集合长度
可以使用 len() 方法获取集合的长度，即获取集合所含的元素的数量。

参数是集合名称，返回值是集合长度。一般来讲返回值是 int 类型。
```PYTHON
>>> test=set()
>>> x0=0
>>> x1=1
>>> test.update({x0,x1})
>>> for i in range(10):
...     x=x0+x1
...     test.add(x)
...     x0=x1
...     x1=x
...
>>> print(test)
{0, 1, 2, 3, 34, 5, 8, 13, 21, 55, 89}
#调用 len() 显示集合长度
>>> len(test)
11
#一个以 len() 为条件的 while 循环
>>> while len(test) > 1:
...     test.pop()
...
0
1
2
3
34
5
8
13
21
55
>>> print('the number left in this set is: ',test)
the number left in this set is:  {89}
#空集的长度是 0 
>>> emptyset=set()
>>> len(emptyset)
0
>>>
```

### 3-4 查询元素和获取内容
我们无法通过引用索引来访问集合(Set)中的项目，因为集合(Set)是无序的，项目没有索引。

我们可以使用关键字 in 和 not in 来检验某个特定元素是否属于给定的集合(Set)。

```PYTHON
>>> bob ={'son','husband','father','employee','intimate','stranger'}
#返回类型是 bool , 语句可以直接完成交互，需要输出结果也可以加print()
>>> 'boyfriend' in bob
False
>>> print('father' in bob)
True
>>>
```

使用 for 循环可以遍历集合(Set)的全部元素:

```PYTHON
>>> s1={1,2,3,4,5,6,7,199,100,2020,1984,56,'56'}
>>> for x in s1:
...     print(x)
...
1984
1
2
3
4
5
6
7
199
100
2020
56
56
>>>
```

使用 print() 函数可以显示集合(Set)的全部内容。关于这个部分，我们在先前的代码里面已经给出了非常多的使用例，因此不多做赘述。

### 3-5 逻辑运算

以下给出的函数，返回值都是一个新集合，参数都是集合(Set), 输入其他类型的参数会报错。

- 交集   intersection()
- 并集   union()
- 差集   difference()
- 对称差集   symmetric_difference()
- 是否交集为空   isdisjoint()
- 是否为给定集合的子集   issubset()
- 是否为包含给定集合   isuperset()

这些函数的性质基本符合数学中对应的定义。因此这里不做详细讲解，而在下面给出这些方法的使用例。读者如果对数学中的集合间的交并补等逻辑运算不熟悉的话，可以自行查阅资料了解。

使用例：

```PYTHON
#下面的运算中，原集合是 x,y，交集是 z ，并集是 z2, x 对 y 的差集是 z1，z3,z4 都是 x,y 的对称差集，z5 是 y 对 x 的差集。
>>> x=set("But I'm a creep")
>>> y=set("I'm a wierdo")
>>> print(x,y)
{'I', "'", 'a', 'B', 't', 'u', 'm', 'c', 'r', 'p', ' ', 'e'} {'I', "'", 'a', 'm', 'i', 'r', 'd', ' ', 'e', 'w', 'o'}
>>> print(len(x),len(y))
12 11
#函数 intersection() 能生成一个新的集合，它是原集合与参数中给定集合的交集
>>> z=x.intersection(y)
>>> print(x)
{'I', "'", 'a', 'B', 't', 'u', 'm', 'c', 'r', 'p', ' ', 'e'}
>>> print(y)
{'I', "'", 'a', 'm', 'i', 'r', 'd', ' ', 'e', 'w', 'o'}
>>> print(len(z),z)
7 {'I', "'", 'a', 'm', 'r', ' ', 'e'}
#intersection() 的返回值类型是集合
>>> type(z)
<class 'set'>
#交集运算符合交换律
>>> z==y.intersection(x)
True
>>> print(z)
{'I', "'", 'a', 'm', 'r', ' ', 'e'}
#差集与并集运算使用例
>>> z1=x.difference(y)
>>> print(z1)
{'B', 't', 'u', 'c', 'p'}
>>> z2=z.union(z1)
>>> print(z2)
{'I', "'", 'a', 'B', 't', 'm', 'u', 'r', 'c', 'p', ' ', 'e'}
#验证 PYTHON 中的差集和并集运算，与数学中的定义的一致性
>>> z2==x
True
#两集合的差集与交集的交集为空集
>>> z.isdisjoint(z1)
True
#任一原集合均是并集的子集
>>> z.issubset(z2)
True
>>> z.issuperset(z2)
False
#一个集合是其自身的子集
>>> z.issubset(z)
True
#空集是任意一个集合的子集
>>> emptyset=set()
>>> emptyset.issubset(z2)
True
#两集合的交集是它们的并集的子集
>>> z2.issuperset(z1)
True
#对称差集满足交换律
>>> z3=x.symmetric_difference(y)
>>> z4=y.symmetric_difference(x)
>>> z3==z4
True
#对称差集是两边得到的差集的并集
>>> print(z3)
{'B', 't', 'i', 'd', 'u', 'c', 'p', 'w', 'o'}
>>> z1=x.difference(y)
>>> z5=y.difference(x)
>>> print(z1)
{'B', 't', 'u', 'c', 'p'}
>>> print(z5)
{'o', 'i', 'w', 'd'}
>>> z3==z1.union(z5)
True
>>>
```

## 4. 掌握集合(Set)可以帮我们做到什么？

***This section is to be fulfilled by the very first guy who saw this* : )**

## 5. 参考文献

*Whoops! I forgot that!*

