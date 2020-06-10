使用\ ``range()``\ 快速初始化数字列表
=====================================

使用\ ``range()``\ 函数能够轻松地生成一系列的数字, 语法格式为:

.. code-block:: python

    range(start, end, step)

表示从\ ``start``\ 指定的数字开始, 到\ ``end``\ 指定的数字结束(不包括该数字), 以\ ``step``\ 为步长, 生成一系列数字.
如果省略\ ``step``\ 参数, 则默认步长为1.

.. note::

    ``start``\ , ``end``\ 和\ ``step``\ 的含义和切片中类似.

需要注意的是, ``range()``\ 函数的返回值并不直接是列表类型, 例如:

.. code-block:: python

    >>> type(range(1, 10))

输出结果为:

.. code-block:: text

    <class 'range'>

可以看到, ``range()``\ 函数的返回值类型\ ``range``\ , 可以通过\ ``list()``\ 或者\ ``tuple()``\ 函数将其转换为列表或元组.

Example:

.. code-block:: python

    list1 = list(range(1, 10))
    
    tuple1 = tuple(range(1, 20))


在实际使用中, ``range()``\ 函数常常和Python循环结构, 推导式一起使用, 几乎能够创建任何需要的数字列表.

Example:

.. code-block:: python

    squares = []
    for value in range(1, 11):
        square = value**2
        squares.append(square)
    print(squares)

