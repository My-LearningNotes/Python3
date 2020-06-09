Python ``print()``\ 函数
=========================


``print()``\ 函数的详细语法格式如下:

.. code-block:: python

    print(value, ..., sep='', end='\n', file=sys.stdout, flush=False)

*   ``value``\ 参数可以接受任意多个变量或值, 因此可以输出多个值;
*   输出多个值时, 默认以空格隔开多个值, 如果希望改变默认的分隔府, 可以通过\ ``sep``\ 参数进行设置;

Example:

.. code-block:: python

    # 设置分隔符
    print('hello', 'world', sep='|')



*   默认情况下, ``print()``\ 函数输出之后总会换行, 这是因为\ ``print()``\ 函数的\ ``end``\ 参数的默认值是\ ``\n``\ , 这个\ ``\n``\ 就代表了换行.
    如果希望\ ``print()``\ 函数输出之后不换行, 则重设\ ``end``\ 参数即可;

Example:

.. code-block:: python
    
    # 设置输出一行后不换行
    print('hello', 'world', end='')


*   ``file``\ 参数指定\ ``print()``\ 函数的输出目标, ``file``\ 参数的默认值是\ ``sys.stdout``\ , 该值默认代表了标准系统输出, 因此\ ``print()``\ 函数默认输出到终端. 
    实际上, 完全可以通过改变该参数让\ ``print()``\ 输出到指定文件中;

Example:

.. code-block:: python

    # 指定输出到文件
    f = open('example.txt', 'w')
    print('hello', 'world', file=f)
    f.close()

*   ``flush``\ 参数用于控制输出缓存, 该参数一般保持为\ ``False``\ 即可, 这样可以获得较好的性能.

