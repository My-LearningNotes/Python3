Python列表添加元素
==================

在实际的开发过程中, 经常需要对Python列表进行更新, 包括向列表中添加元素, 修改列表中的元素以及删除元素.

可以使用\ ``+``\ 运算符将多个序列拼接起来, 生成一个新的序列, 原有的序列不会改变.
虽然可以使用\ ``+``\ 运算符往列表中通过添加元素, 但\ ``+``\ 更多的是用来进行拼接操作, 而且执行效率不高, 所以通常不用它来往列表中添加元素.


``append()``\ 方法添加元素
--------------------------

``append()``\ 方法用于在列表的尾部追加元素, 其语法格式为:

.. code-block:: python

    # listName表示要添加元素的列表
    # obj表示要添加到列表尾部的数据, 它可以是单个元素, 也可以是列表, 元组等.
    listName.append(obj)

Example:

.. code-block:: python

    list1 = ['Python', 'C++', 'Java']
    # 追加元素
    list1.append('Rust')

    # 追加元组, 整个元组被当成一个元素
    tuple1 = ('JavaScript', 'Go')
    list1.append(tuple1)

    # 追加列表, 整个列表也被当成一个元素
    list1.append(['Ruby', 'SQL'])


``extend()``\ 方法添加元素
--------------------------

``extend()``\ 和\ ``append()``\ 的不同之处在于: ``extend()``\ 不会把列表或元组作为一个整体, 而是把它们包含的元素逐个添加到列表中.

``extend()``\ 方法的语法格式如下:

.. code-block:: python

    # listName是要添加元素的列表
    # obj是要添加到列表尾部的数据, 它可以是单个元素, 也可以是列表, 元组等.
    listName.extend(obj)

Example:

.. code-block:: python

    list1 = ['Python', 'C++', 'Java']
    # 追加元素
    list1.append('Rust')

    # 追加元组, 元组被拆分成多个元素
    tuple1 = ('JavaScript', 'Go')
    list1.extend(tuple1)

    # 追加列表, 列表也被拆分成多个元素
    list1.extend('Ruby', 'SQL')


``insert()``\ 方法插入元素
--------------------------

``append()``\ 和\ ``extend()``\ 方法只能在列表尾部插入元素, 如果希望在列表中间某个位置插入元素, 那么可以使用\ ``insert()``\ 方法.

``insert()``\ 的语法格式为:

.. code-block:: python

    # index表示位置的索引值
    # 将obj插入到listName列表的第index个元素的位置
    listName.insert(index, obj)

当插入列表或元组时, ``insert()``\ 也会将它们作为一个整体, 作为一个元素插入到列表中, 这一点和\ ``append()``\ 是一样的.

Example:

.. code-block:: python

    list1 = ['Python', 'C++', 'Java']
    # 插入元素
    list1.insert(1, 'Rust')

    # 插入元组, 整个元组被当成一个元素
    tuple1 = ('JavaScript', 'Go')
    list1.insert(2, tuple1)

    # 插入列表, 整个列表被当成一个元素
    list1.insert(3, ['Ruby', 'SQL'])

    # 插入字符串, 整个字符串被当成一个元素
    list1.insert(0, 'hello, world')

