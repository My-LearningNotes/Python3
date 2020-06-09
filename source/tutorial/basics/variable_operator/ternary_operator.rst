Python三目运算符
================

Python中提供的三目运算符类似于其它语言中的\ ``?:``\ , 是一个\ **条件运算符**\ :

.. code-block:: python
    :emphasize-lines: 1

    expr1 if condition else expr2

``condition``\ 是判断条件, ``expr1``\ 和\ ``expr2``\ 是两个表达式. 
如果\ ``condition``\ 成立, 就执行\ ``expr1``\ , 并把\ ``expr1``\ 的结果作为整个表达式的结果; 
如果\ ``condition``\ 不成立, 就执行\ ``expr2``\ , 并把\ ``expr2``\ 的结果作为整个表达式的结果.


例如, 现在有两个数字, 我们希望获得其中较大的那一个, 那么可以使用\ ``if else``\ 语句:

.. code-block:: python

    if a > b:
        max = a
    else:
        max = b

使用条件运算符更加简洁:

.. code-block:: python

    max = a if a > b else b

上面的语句的含义是:

    *   如果\ ``a > b``\ 成立, 就把\ ``a``\ 作为整个表达式的值, 并赋值给变量\ ``max``\ ;
    *   如果\ ``a > b``\ 不成立, 就把\ ``b``\ 作为整个表达式的值， 并赋值给变量\ ``max``\ .


三目运算符的嵌套
----------------

Python三目运算符支持嵌套, 如此可以构成更加复杂的表达式. 
在嵌套时需要注意\ ``if``\ 和\ ``else``\ 的配对, 例如:

.. code-block:: python

    a if a>b else c if c>d else d
    
应该理解为:

.. code-block:: python

    a if a>b else (c if c>d else d)


.. note::

    三目运算符虽然可以嵌套, 但是嵌套后通常理解起来比较困难, 所以尽量不要嵌套.

