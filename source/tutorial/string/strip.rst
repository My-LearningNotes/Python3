去除字符串中的空格
==================

用户输入数据时, 很有可能会无意中输入多余的空格, 或者在一些场景中, 字符串前后不允许出现空格和特殊字符, 此时就需要去除字符串中的空格和特殊字符.

.. note::

    这里所说的特殊字符, 指的是水平制造表符\ ``\t``\, 回车符\ ``\r``\ , 换行符\ ``\n``\ 等.

在Python中, 字符串变量提供了3种方法用来删除字符串中的多余的空格和特殊字符, 它们分别是:

*   ``strip()`` - 删除字符串前后(左右两侧)的空格和特殊字符;
*   ``lstrip()`` - 删除字符串前面(左边)的空格和特殊字符;
*   ``rstrip()`` - 删除字符串后面(右边)的空格和特殊字符.

注意, Python字符串是不可变的, 因此这三个方法指是返回字符串前面或后面空白被删除之后的副本, 并不会改变字符串本身.


``strip()``\ 方法
-----------------

``strip()``\ 方法用于删除字符串左右两边的空格和特殊字符, 该方法的语法格式为:

.. code-block:: python
    :emphasize-lines: 1

    string.strip([chars])

``[chars]``\ 用来指定要删除的字符, 可以同时指定多个, 如果没有指定, 则默认会删除空格以及制表符, 回车符, 换行符等特殊字符.


``lstrip()``\ 方法
------------------

``lstrip()``\ 方法用于去掉字符串左侧的空格和特殊字符.

.. code-block:: python
    :emphasize-lines: 1

    string.lstrip([chars])


``rstrip()``\ 方法
------------------

``rstrip()``\ 方法用于删除字符串右侧的空白和特殊字符.

.. code-block:: python
    :emphasize-lines: 1

    string.rstrip([chars])


