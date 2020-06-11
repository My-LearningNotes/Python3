Python字典方法
==============

Python字典的数据类型为\ ``dict``\ , 可以使用\ ``dir(dict)``\ 来查看该类型包含那些方法.


``keys()``, ``values()``\ 和\ ``items()``\ 方法
-----------------------------------------------

这三个方法都用来获取字典中的特定数据:

    *   ``keys()``\ 方法用于返回字典中的所有键(key);
    *   ``values()``\ 方法用于返回字典中的所有值(value);
    *   ``items()``\ 方法用于返回字典中所有的键值对(key-value).


在Python 3.x中, 如果想使用这三个方法返回的数据, 一般有两种方案:

*   使用\ ``list()``\ 函数, 将它们返回的数据转换为列表

Example:

.. code-block:: python

    scores = dict(语文=99, 数学=100, 英语=100)
    print(list(scores.keys())
    print(list(scores.values())
    print(list(scores.items())


*   使用\ ``for in``\ 循环遍历它们的返回值

Example:

.. code-block:: python

    scores = dict(语文=99, 数学=100, 英语=100)

    # 遍历keys
    for key in scores.keys():
        print(key)

    # 遍历values
    for value in scores.values():
        print(value)

    # 遍历items
    for key, value in scores.items():
        print(f'key: {key}, value: {value}')

    # 遍历字典, 默认是遍历keys
    for key in scores:
        print(key)


``copy()``\ 方法
----------------

``copy()``\ 方法返回一个字典的拷贝.

.. attention::

    对于字典的\ ``copy()``\ 方法, 字典中的可变类型的值(列表, 字典), 执行的是浅拷贝.

Example:

.. code-block:: python

    a = {'one': 1, 'two': 2, 'three': [1, 2, 3]}
    b = a.copy()
    # 向a中添加新的键值对, 不会影响b
    a['four'] = 4
    print(a)
    print(b)

    # b和a共享[1, 2, 3](浅拷贝), 因此a中对该列表的修改会影响b
    a['three'].remove(1)
    print(a)
    print(b)


``update()``\ 方法
------------------

``update()``\ 方法可以使用一个字典所包含的键值对来更新已有的键值对.

在执行\ ``update()``\ 方法时, 如果被更新的字典中已包含对应的键值对, 那么原value会被覆盖; 
如果被更新的字典中不包含对应的键值对, 则该键值对被添加进去.

Example:

.. code-block:: python

    dict1 = {'one': 1, 'two': 2, 'three': 3}
    print(dict1)

    # 使用update()更新字典
    dict1.update({'one': 100, 'four': 4})
    print(dict1)


``pop()``\ 和\ ``popitem()``\ 方法
----------------------------------

``pop()``\ 和\ ``popitem()``\ 都用来删除字典中的键值对, 
不同的是, ``pop()``\ 用来删除指定的键值对, 而\ ``popitem()``\ 用来随机删除一个键值对, 它们的语法格式如下:

.. code-block:: python

    dictName.pop(key)
    dictName.popitem()

Example:

.. code-block:: python

    scores = {'语文': 98, '数学': 99, '英语': 100, '物理': 99, '化学': 98, '生物': 100}
    print(scores)

    # 根据key删除指定的键值对
    scores.pop('数学')
    print(scores)

    # 随机删除一个键值对
    scores.popitem()
    print(scores)

.. note::

    对\ ``popitem()``\ 的说明

    说\ ``popitem()``\ 随机删除字典中的一个键值对是不准确的, 虽然字典是无序的, 但键值对在底层也是有存储顺序的, 
    ``popitem()``\ 总是弹出底层中的最后一个键值对, 这和列表的\ ``pop()``\ 方法类似, 都实现了数据结构中的"出栈"操作.


``setdefault()``\ 方法
----------------------

``setdefault()``\ 方法用来给指定的键设置一个默认值, 其语法格式为:

.. code-block:: python

    dictName.setdefault(key[, defaultValue])

``defaultValue``\ 表示默认值, 省略的话, 默认是\ ``None``\ .

*   若指定的key已经存在, ``setdefault()``\ 什么都不做, 直接返回该key对应的value;
*   若指定的key不存在, 则\ ``setdefault()``\ 为该key设置指定的默认值, 并返回该默认值.

Example:

.. code-block:: python

    scores = {'语文': 98, '数学': 99, '英语': 100}
    print(scores)

    # key不存在, 指定默认值
    scores.setdefault('物理', 100)
    print(scores)

    # key不存在, 不指定默认值
    scores.setdefault('化学')
    print(scores)

    # key已存在, 指定默认值
    scores.setdefault('数学', 100)
    print(scores)

