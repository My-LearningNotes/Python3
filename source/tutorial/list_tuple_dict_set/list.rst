Python列表(list)
================


创建列表
--------

在Python中, 创建列表的方法有两种.

*   使用\ ``[]``\ 指将创建列表

使用\ ``[]``\ 创建列表之后, 一般使用\ ``=``\ 将它赋值给某个变量, 具体格式如下:

.. code-block:: python

    listname = [element1, element2, ..., elementN]
    
其中, ``listname``\ 表示变量名, ``element1~elementN``\ 表示列表元素.

另外, 使用此方式创建列表时, 列表中元素可以有多个, 也可以一个都没有, 例如:

.. code-block:: python

    emptyList = []

这表明, ``emptyList``\ 是一个空列表.


*   使用\ ``list()``\ 函数创建列表

除了使用\ ``[]``\ 创建列表外, Python还提供了一个内置函数\ ``list()``\ , 使用它可以将其它数据类型(可迭代类型)转换为列表类型.

Example:

.. code-block:: python

    # 将字符串转换为列表
    list1 = list('hello')

    # 将元组转换为列表
    tuple1 = ('Python', 'Java', 'C++')
    list2 = list(tuple1)

    # 将字典转换为列表
    dict1 = {'a': 100, 'b': 200, 'c': 300}
    list3 = list(dict1)

    # 将区间转换为列表
    range1 = range(1, 6)
    list4 = list(range1)

    # 创建空列表
    list5 = list()


访问列表元素
------------

*   使用索引(index)访问列表中的某一个元素: ``listName[index]``\ ;
*   使用切片(slice)访问列表中的一组元素: ``listName[start:stop:step]``\ .


删除列表
--------

对于已经创建的列表, 如果不再使用, 可以使用\ ``del``\ 关键字将其删除.

实际开发中并不经常使用\ ``del``\ 来删除列表, 因为Python自带的垃圾回收机制会自动销毁吴用的列表, 即使开发者不手动删除, Python也会自动将其回收.

``del``\ 关键字的语法为:

.. code-block:: python

    del listName

``listName``\ 表示要删除的列表名.

