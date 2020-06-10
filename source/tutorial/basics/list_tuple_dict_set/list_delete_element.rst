Python列表删除元素
==================

在Python列表中删除元素主要分为以下三种场景:

    *   根据目标元素所在的位置的索引进行删除, 可以使用\ ``del``\ 关键字或者\ ``pop()``\ 方法;
    *   根据元素本身的值进行删除, 可以使用列表的\ ``remove()``\ 方法;
    *   将列表中的所有元素全部删除, 可以使用列表的\ ``clear()``\ 方法.


``del``\ : 根据索引值删除元素
-----------------------------

``del``\ 是Python中的关键字, 专门用来执行删除操作, 它不仅可以删除整个列表, 还可以删除列表中的某个元素.

``del``\ 可以删除列表中的单个元素, 语法格式为:

.. code-block:: python

    # listName表示列表名称
    # index表示元素索引值, 可以使用负索引
    del listName[index]

Example:

.. code-block:: python

    lang = ['Python', 'C++', 'Java', 'PHP', 'Ruby', 'Matlab']

    # 使用整数索引
    del lang[2]

    # 使用负数索引
    del lang[-2]


``del``\ 可以删除一组元素, 语法格式(类似于切片)为:

.. code-block:: python

    # start表示起始索引
    # end表示结束索引
    # step表示步长
    # 以step为步长, 删除从索引start到end之间的元素, 不包括end位置的元素
    del listName[start:end:step]

Example:

.. code-block:: python

    lang = ['Python', 'C++', 'Java', 'PHP', 'Ruby', 'Matlab']

    del lang[0:4:2]

    lang.extend(['SQL', 'C#', 'Go'])
    del lang[-5:-2:2]


``pop()``\ 根据索引值删除元素
-----------------------------

``pop()``\ 方法用来删除列表中指定索引处的元素, 语法格式为:

.. code-block:: python

    listName.pop(index)

如果省略\ ``index``\ 参数, 默认会删除列表中的最后一个元素(类似于栈数据结构的"出栈"操作).

Example:

.. code-block:: python

    nums = [0, 1, 2, 3, 4, 5, 6]

    # 删除索引为3的元素
    nums.pop(3)

    # 删除最后一个元素
    nums.pop()


大部分编程语言都会提供和\ ``pop()``\ 相对应的方法, 就是\ ``push()``\ , 该方法用来将元素添加到列表的尾部, 类似于栈数据结构中的"入栈"操作. 
但是Python是个例外, 并没有提供\ ``push()``\ 方法, 因为完全可以使用\ ``append()``\ 来代替\ ``push()``\ 的功能.


``remove()``\ : 根据元素值进行删除
----------------------------------

``remove()``\ 方法会根据元素自身的值进行删除操作.

``remove()``\ 方法只会删除第一个和指定值相同的元素, 而且必须保证该元素是存在的, 否则会引发\ ``ValueError``\ 异常.

Example:

.. code-block:: python

    nums = [10, 20, 30, 20, 40, 50]
    # 第一次删除20
    nums.remove(20)

    # 第二次删除20
    nums.remove(20)

    # 删除100
    # 因为没有这样的元素, 引发ValueError异常
    nums.remove(100)

因为使用\ ``remove()``\ 删除部存在的元素时会引发异常, 所以在使用\ ``remove()``\ 删除元素时最好提前判断一下.


``clear()``\ : 删除列表中的所有元素
-----------------------------------

列表的\ ``clear()``\ 方法用来删除列表中的所有元素, 即清空列表.

