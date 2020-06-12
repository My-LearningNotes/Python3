合并字符串
==========

``join()``\ 方法用来将列表或元组中包含的多个字符串采用固定的分隔符连接在一起.

.. code-block:: python

    newStr = sep.joint(iterable)

*   ``newStr``\ 表示合并后生成的新字符串;
*   ``sep``\ 用于指定合并时的分隔符;
*   ``iterable``\ 表示做合并操作的源字符串数据, 允许以列表, 元组等形式提供.

Example:

.. code-block:: python

    dir = ['', 'usr', 'bin', 'env']
    print('/'.join(dir))

输出结果为:

.. code-block:: python

    /usr/bin/env

