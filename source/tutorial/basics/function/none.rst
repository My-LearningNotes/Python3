``None``
=========

在Python中, 有一个特殊的常量\ ``None``\ , 表示没有值, 也就是空值.

这里的空值并不代表空对象, 即\ ``None``\ 和\ ``[]``, ``''``\ 不同:

.. code-block:: python

    >>> None is []
    False
    >>> None is ''
    False

``None``\ 有自己的数据类型:

.. code-block:: python

    >>> type(None)
    <class 'NoneType'>

需要注意, ``None``\ 是\ ``NoneType``\ 数据类型的唯一值, 
也就是说, 我们不能再创建其它\ ``NoneType``\ 类型的变量, 但是可以将\ ``None``\ 赋值给任何变量. 
如果希望变量中存储的东西不与任何其它值混淆, 就可以使用\ ``None``\ .

