Python元组(tuple)
=================

元组(tuple)是Python中另一个重要的序列结构, 和列表类似, 元组也是由一系列按特定顺序排序的元素组成的.

元组和列表的不同之处在于:

    *   列表的元素是可以更改的, 包括修改元素值, 删除和插入元素, 所以列表是可变序列;
    *   而元组一旦被创建, 它的元素就不可更改了, 所以元组是不可变序列.
      
元组也可以看作是不可变的列表, 通常情况下, 元组用于保存无需修改的内容.

从形式上看, 元组的所有元素都放在一对小括号\ ``()``\ 中, 元素之间用逗号\ ``,``\ 分隔, 如下所示:

.. code-block:: python

    (element1, element2, ..., elementN)

其中\ ``element1~elementN``\ 表示元组中的各个元素, 个数没有限制, 只要是Python支持的数据类型就可以.

从存储内容上看, 元组可以存储整数, 实数, 字符串, 列表, 元组等任何类型的数据, 并且在同一个元组中, 元素的类型可以不同.


创建元组
--------

Python提供了两种创建元组的方法.

使用\ ``()``\ 直接创建
^^^^^^^^^^^^^^^^^^^^^^

通过\ ``()``\ 创建元组后, 一般使用\ ``=``\ 将其赋值给某个变量, 具体格式为:

.. code-block:: python

    tupleName = (element1, element2, ..., elementN)

在Python中, 元组通常都是使用一对小括号将所有元素包围起来, 但小括号不是必须的, 只要将各元素用逗号隔开, Python就会将其是位元素.

需要注意的是, 当创建的元组中只有一个元素时, 该元素后面必须要加一个逗号\ ``,``\ , 否则Python解释器不会将其视为元组.

Example:

.. code-block:: python

    # 最后加上逗号
    a = ('hello, world',)
    print(type(a))

    # 最后不加逗号
    b = ('hello, world')
    print(type(b))

运行结果为:

.. code-block:: text

    <class 'tuple'>
    <class 'str'>


使用\ ``tuple()``\ 创建元组
^^^^^^^^^^^^^^^^^^^^^^^^^^^

除了使用\ ``()``\ 创建元组外, Python还提供了一个内置的函数\ ``tuple()``\ , 用来将其它数据类型转换为元组类型.

``tuple()``\ 的语法格式如下:

.. code-block:: python

    tuple(data)

其中, ``data``\ 表示可以转换为元组的数组, 包括字符串, 列表, range对象等(可迭代对象).

Example:

.. code-block:: python

    # 将字符串转换为元组
    tuple1 = tuple('hello')

    # 将列表转换为元组
    list1 = ['Python', 'Java', 'C++']
    tuple2 = tuple(list1)

    # 将字典转换为元组
    dict1 = {'a': 100, 'b': 200}
    tuple3 = tuple(dict1)

    # 将区间转换为元组
    range1 = range(1, 6)
    tuple4 = tuple(range1)

    # 创建空元组
    tuple5 = tuple()


访问元组元素
------------

和列表一样, 可以使用索引(index)访问元组中的某个元素, 也可以使用切片访问元组中的一组数据(得到的是一个新的子元组).


修改元组
--------

元组是不可变序列, 元组中的元素不能被修改, 只能创建一个新的元组去替代旧的元组.

*   对元组变量重新赋值;
*   通过拼接的方式向元组中添加新元素(生成一个新的元素).


删除元组
--------

当创建的元组不再使用时, 可以通过\ ``del``\ 关键字将其删除.


总结
----

总的来说, 元组确实没有列表那么多功能, 但是元组依旧是很重要的序列类型之一, 元组的不可替代性体现在以下这些场景中:

    *   元组作为很多内置函数和序列类型方法的返回值存在, 也就是说, 在使用某些函数或者方法时, 它的返回值是元组类型, 因此你必须对元组进行处理;
    *   元组比列表的访问和处理速度更快, 因此, 当需要对指定元素进行访问, 且不涉及修改元素的操作时, 建议使用元组;
    *   元组可以在映射中当作"键"使用, 而列表不行.

