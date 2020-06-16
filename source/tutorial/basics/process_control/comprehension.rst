Python推导式
============

推导式, 是Python特有的一种特性. 
使用推导式可以快速生成列表, 元组, 字典以及集合类型, 因此推导式又可以细分为列表推导式, 元组推导式， 字典推导式和集合推导式.


列表推导式
----------

列表推导式可以利用range区间, 元组, 列表, 字典和集合等数据类型, 快速生成一个指定的列表.

列表推导式的语法格式如下:

.. code-block:: python
    :emphasize-lines: 1

    [expression for var in obj]

其执行顺序如下:

.. code-block:: python

    for var in obj:
        expression

可以这么理解: 它是对\ ``for``\ 循环语句的格式做了一个简单的变形, 并用\ ``[]``\ 括起来, 只不过最大的不同之处在于, 
列表推导式最终会将循环过程中计算得到的一系列值组成一个列表.

Example:

.. code-block:: python

    l = [x ** 2 for x in range(0, 10)]
    print(l)

    运行结果为:
    [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

还可以在列表推导式中添加\ ``if``\ 条件语句, 这样列表推导式将只迭代那些符合条件的元素.

.. code-block:: python
    :emphasize-lines: 1

    [expression for var in obj if condition]

其执行顺序为:

.. code-block:: python

    for var in obj:
        if condition:
            expression

Example:

.. code-block:: python

    l = [x ** 2 for x in range(0, 10) if x % 2 == 0]
    print(l)

    运行结果为:
    [0, 4, 16, 36, 64]

实际上, 在列表推导式中还可以嵌套循环和条件语句, 它们的执行顺序是按照它们的定义顺序, 一层层的嵌套.

Example:

.. code-block:: python

    a = [20, 12, 66, 34, 39, 78, 36, 57, 121]
    b = [3, 5, 7, 11]
    result = [(x, y) for x in a for y in b if x % y == 0]
    print(result)

    运行结果为:
    [(30, 3), (30, 5), (12, 3), (66, 3), (66, 11), (39, 3), (78, 3), (36, 3), (57, 3), (121, 11)]

其中列表推导式的执行顺序为:

.. code-block:: python

    for x in a:
        for y in b:
            if x % y == 0:
                ...


元组推导式
----------

元组推导式的语法格式如下:

.. code-block:: python
    :emphasize-lines: 1

    (expression for var in obj)

元组推导式和列表推导式唯一的不同是: 列表推导式使用\ ``[]``\ 将各部分括起来, 而元组推导式使用的是\ ``()``\ .

Example:

.. code-block:: python

    a = (x for x in range(1, 10))
    print(a)

    运行结果为:
    <generator object <genexpr> at 0x7fc9dc3c5eb8>

从上面的执行结果可以看到, **使用元组推导式生成的结果并不是一个元组, 而是一个生成器对象, 这一点和列表推导式是不同的.**

如果想要使用元组推导式获得新元组或新元组中的元素, 有以下三种方式:

    *   使用\ ``tuple()``\ 函数, 可以直接将生成器对象转换为元组.

    Example:

    .. code-block:: python

        a = (x for x in range(1, 10))
        print(tuple(a))
        
        运行结果为:
        (1, 2, 3, 4, 5, 6, 7, 8, 9)

    *   直接使用\ ``for``\ 循环遍历生成器对象, 可以获得各个元素.

    Example:

    .. code-block:: python

        a = (x for x in ranage(1, 10))
        for i in a:
            print(i, end=' ')
        
        运行结果为:
        1 2 3 4 5 6 7 8 9

    *   使用\ ``__next__()``\ 方法遍历生成器对象, 也可以获得各个元素.

    Exmaple:

    .. code-block:: python

        a = (x for x in range(3))
        print(a.__next__(), end=' ')
        print(a.__next__(), end=' ')
        print(a.__next__(), end=' ')

        运行结果为:
        0 1 2


字典推导式
----------

在Python中, 使用字典推导式可以借助列表, 元组, 字典, 集合以及range区间, 快速生成符合需求的字典.

字典推导式的语法格式如下:

.. code-block:: python
    :emphasize-lines: 1

    {expression for var in obj}

可以看到, 和其它推导式相比, 唯一的不同在于, 字典推导式用的是大括号\ ``{}``\ .

.. note::

    在字典推导式中, 表达式应该是\ ``key: value``\ 的形式.

Example:

.. code-block:: python

    l = ['hello', 'world']
    d = {x: len(x) for x in l}
    print(d)

    运行结果为:
    {'hello': 5, 'world': 5}


集合推导式
----------

在Python中, 使用集合推导式可以借助列表, 元组, 字典, 集合以及range区间, 快速生成符合需求的集合.

集合推导式的语法格式和字典推导式完全相同. 
那么, 给定一个推导式, 如何判断是字典推导式还是集合推导式呢?
最简单直接的方式, 就是根据表达式进行判断, 如果表达式以键值对(``key: value``)的形式, 则证明此推导式是字典推导式, 反之则是集合推导式.

Example:

.. code-block:: python

    a = (1, 1, 2, 3, 4, 4)
    s = {x for x in a)
    print(s)

    运行结果为:
    {1, 2, 3, 4}

