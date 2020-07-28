Python字符串拼接
================

在Python中拼接字符串很简单, 直接将两个字符串紧挨着写在一起即可, 具体格式为:

.. code-block:: python

    strName = "str1""str2"

但是这种方法只能拼接字符串常量, 如果需要使用变量, 就要使用\ ``+``\ 运算符来拼接, 具体格式为:

.. code-block:: python

    strName = str1 + str2

当然, ``+``\ 运算符也能拼接字符串常量.


字符串和数字的拼接
------------------

Python不允许直接拼接字符串和数字, 需要先将数字转换成字符串. 

可以使用\ ``str()``\ 和\ ``repr()``\ 函数将数字转换为字符串, 它们的使用格式为:

.. code-block:: python

    str(obj)
    repr(obj)

``obj``\ 表示要转换的对象, 它可以是数字, 列表, 元组, 字典等多种类型的数据.


``str()``\ 和\ ``repr()``\ 的区别:

    *   ``str()``\ 用于将数据转换成适合人类阅读的字符串形式;
    *   ``repr()``\ 用于将数据转换成适合解释去阅读的字符串形式(用引号将字符串包围起来, 这就是Python字符串的表达式形式).

Example:

.. code-block:: python

    msg = 'hello, world'
    msg_str = str(msg)
    msg_repr = repr(msg)

    print(type(msg_str))
    print(msg_str)

    print(type(msg_repr))
    print(msg_repr)

输出结果为:

.. code-block:: text

    <class 'str'>
    hello, world
    <class 'str'>
    'hello, world'

