Python列表查找元素
==================

Python列表提供了\ ``index()``\ 和\ ``count()``\ 方法用来查找元素.


``index()``\ 方法
-----------------

``index()``\ 方法用来查找某个元素在列表中出现的位置(也就是索引), 如果该元素不存在, 则会引发\ ``ValueError``\ 异常, 所以在查找之前最好使用\ ``count()``\ 方法判断一下.

``index()``\ 的语法格式为:

.. code-block:: python

    # listName - 列表名称
    # obj - 要查找的元素
    # start - 起始位置
    # end - 结束位置
    listName(obj, start, end)

``start``\ 和\ ``end``\ 参数用来指定检索范围:

    *   ``start``\ 和\ ``end``\ 可以省略, 表示检索整个列表;
    *   如果只写\ ``start``\ 不写\ ``end``\ , 那么表示检索从\ ``start``\ 到末尾的元素;
    *   如果\ ``start``\ 和\ ``end``\ 都写, 那么表示检索\ ``start``\ 和\ ``end``\ 之间的元素.

Example:

.. code-block:: python

    nums = [40, 36, 89, 2, 36, 100, 7, -20.5, -999]

    #检索列表中的所有元素
    print(nums.index(2))
    #检索3~7之间的元素
    print(nums.index(100, 3, 7))
    #检索4之后的元素
    print(nums.index(7, 4))
    #检索一个不存在的元素
    print(nums.index(55))


``count()``\ 方法
-----------------

``count()``\ 方法用来统计某个元素在列表中出现的次数, 语法格式为:

.. code-block:: python

    # listName - 列表名称
    # obj - 要统计的元素
    listName.count(obj)

如果\ ``count()``\ 返回0, 就表示列表中不能存在该元素, 所以\ ``count()``\ 也可以用来判断列表中是否包含某个元素.

Example:

.. code-block:: python

    nums = [10, 20, 30, 40, 50, 10, 10]
    # 统计值出现的次数
    print('10出现了{}次'.format(nums.count(10)))

    # 判断一个值是否存在
    if nums.count(100):
        print('列表中存在100这个元素')
    else:
        print('列表中不存在100这个元素')

