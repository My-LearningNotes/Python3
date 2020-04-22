条件和循环语句
==============


赋值
----


序列解包
~~~~~~~~

**序列解包(或可迭代对象解包): 将一个序列(或任何可迭代对象)解包, 并将得到的值存储到一系列变量中.**

.. note::

    解包, 可以简单的理解为取出各个元素.

Example:

.. code-block:: python

    x, y, z = 1, 2, 3
   
    values = 4, 5, 6
    x, y, z = values

-   要解包的序列包含的元素个数必须与等号左边列出的目标个数相同, 否则将引发异常;

-   可以使用星号运算符(\ ``*``)来收集多余的值, 这样就无需确保值和变量的个数相同;

Example:

.. code-block:: python

    a, b, *reset = 1, 2, 3, 4, 5

-   带\ ``*``\ 的变量不一定要放在末尾位置，还可以放在其它位置.

Example:

.. code-block:: python

    *a, b, c, d = 1, 2, 3, 4, 5, 6

.. note::

    将相应位置的值赋给相应位置的变量，多余的值被收集.


链式赋值
~~~~~~~~

链式赋值是一种快捷方式, 用于将多个变量绑定到同一个值.

Example:

.. code-block:: python

    x = y = someFunction()


增强赋值
~~~~~~~~

就是C/C++中复合赋值运算符.

使用复合赋值运算符, 可让代码更紧凑, 更简洁, 同时在很多情况下的可读性更强.


代码块
------

-   代码块是一组语句;

-   在Python中, 使用相同的缩进来定义代码块;

    .. note::

        在C/C++中，使用花括号定义代码块.

        在很多语言中，都使用一个特殊的单词或字符(如\ ``begin``\ 或\ ``{``)来标识代码块的起始位置，并使用一个特殊的单词或字符(如\ ``end``\ 或\ ``}``)来标识结束位置。

-   在Python中，使用冒号(\ ``:``)指出接下来的是一个代码块, 并将该代码块中的每行代码都做相同的缩进.


条件和条件语句
--------------

-   用作布尔表达式时，下面的值都将被视为\ **假**: ``False``, ``None``, ``0``, ``()``, ``[]``, ``''``, ``{}``;

-   其它各种值都被视为\ **真**.


``if``\ 条件语句
~~~~~~~~~~~~~~~~

.. code-block:: python

    if condition:
        code-block


``else``\ 子句
~~~~~~~~~~~~~~

.. code-block:: python

    if confition:
        code-block_1
    else:
        code-block_2


``elif``\ 子句
~~~~~~~~~~~~~~

``elif``\ 是\ ``else if``\ 的缩写，是包含条件的\ ``else``\ 子句.

.. code-block:: python

    if condition_1:
   	    code-block_1
    elif condition_2:
   	    code-block_2
    else:
   	    code-block_3


条件语句的嵌套
~~~~~~~~~~~~~~

条件语句可以嵌套.


断言
~~~~

-   **断言**\ 就是对给定的条件进行判断, 如果条件不成立则立即使程序崩溃;

-   在Python中，断言使用关键字\ ``assert``;

-   可在程序中添加\ ``assert``\ 语句充当检查点, 这很有帮助;

-   可在\ ``assert``\ 后添加一个字符串, 作为对断言的说明;

Example:

.. code-block:: python

    age = -1
    assert 0 < age < 100, 'The age must be realistic'


循环
----


``while``\ 循环
~~~~~~~~~~~~~~~

.. code-block:: python

    while condition:
        code-block

``while``\ 循环用来\ **对给定的条件进行判断, 决定是否执行代码块**\ .


``for``\ 循环
~~~~~~~~~~~~~

``for``\ 循环用来\ **进行迭代操作**\ .

.. note::

    迭代，可以理解为依次取出可迭代对象中的各个元素.

Example:

.. code-block:: python

    for number in range(1, 101):
        print(number)

.. note::

    只要能够使用\ ``for``\ 循环, 就不要使用\ ``while``\ 循环.


迭代字典
~~~~~~~~

要遍历字典的所有关键字, 可像遍历序列那样使用普通的\ ``for``\ 语句.

-   对\ ``keys``\ 进行迭代

对字典迭代，默认就是对\ ``keys``\ 迭代.

-   对\ ``values``\ 进行迭代

对\ ``d.values()``\ 迭代.

-   对\ ``items``\ 进行迭代

对\ ``d.items()``\ 迭代.

Example:

.. code-block:: python

    d = {'x': 1, 'y': 2, 'z': 3}
    for key in d:
        print(key, 'corresponds to', d[key])
      
    for value in d.values():
        print(value)
   
    for key, value in d.items():
        print(key, value)


一些可迭代工具
~~~~~~~~~~~~~~


并行迭代
^^^^^^^^

有时候, 我们会想\ **同时迭代两个序列(即同时获取两个序列的元素)**\ :

-   可以使用索引来实现

Example:

.. code-block:: python

    names = ['anne', 'beth', 'george', 'damon']
    ages = [12, 45, 32, 102]
   
    for i in range(len(names)):
        print(names[i], ages[i])

-   可以使用内置函数\ ``zip``

   -   它将两个序列"缝合"起来，返回一个由元组组成序列

   -   返回值是一个可迭代对象，在"缝合"后，可以在迭代中将元组解包，从而实现对两个序列的并行迭代

Example:

.. code-block:: python

    names = ['anne', 'beth', 'george', 'damon']
    ages = [12, 45, 32, 102]
    for name,age in zip(names, ages):
   	    print(name, age)

-   函数\ ``zip``\ 可以"缝合"任意长度的序列

需要指出的是，当序列的长度不同时，函数\ ``zip``\ 将在最短的序列用完后停止"缝合".


迭代时获取索引
^^^^^^^^^^^^^^

在有些情况下, 我们需要在迭代对象序列的同时获取当前对象的索引．

-   可定义一个索引并初始化为0，每次迭代时将索引加1

Example:

.. code-block:: python

    names = ['anne', 'beth', 'george', 'damon']

    index = 0
    for name in names:
       print('{}: {}'.format(index, name))
       index += 1

\* 使用内置函数\ ``enumerate``

使用\ ``enumerate``\ , 让我们可以在迭代时同时获得序列的索引和值

Example:

.. code-block:: python

   names = ['anne', 'beth', 'george', 'damon']
   for index, name in enumerate(names):
       print(index, name)


反向迭代和排序后再迭代
^^^^^^^^^^^^^^^^^^^^^^

两个内置函数\ ``reversed``\ 和\ ``sorted``\ , 它们类似列表方法\ ``reverse``\ 和\ ``sort``\ , 但可用于任何序列或可迭代对象, 且不修改原对象, 而是返回反转和排序后的版本.

可以使用这两个内置函数, 对对象排序或反转后再迭代.


跳出循环
~~~~~~~~

-   ``break``

要结束(跳出)循环，可使用\ ``break``.

-   ``continue``

要结束当前迭代，并跳到下一次迭代的开头，可使用\ ``continue``.

-   ``while True/break``\ 实例

Example:

.. code-block:: python

    while True:
   	    word = input('Please enter a word:')
   	    if not word:
   		    break
   	    print('The word was', word)

另一种写法是:

.. code-block:: python

    word = input('Please enter a word:')
    while word:
       print('The word was', word)a
       word = input('Please enter a word')

上面的这种实现中包含重复代码，没有使用\ ``while True/break``\ 好.

.. note::

    消灭重复代码.


循环中的\ ``else``\ 子句
~~~~~~~~~~~~~~~~~~~~~~~~

通常, 在循环中使用\ ``break``\ 是因为你"发现"了什么或"出现"了什么情况.

要在循环提前结束时采取某种措施很容易(先做提前结束的处理，再\ ``break``), 但有时候你可能想在循环正常结束时才采取某种措施.

如果判断循环是提前结束还是正常结束呢?
可在循环开始前定义一个布尔变量并将其设置为\ ``False``\ , 再在跳出循环时将其设置为\ ``True``\ .
这样就可以在循环后面使用一条\ ``if``\ 语句来判断循环是否是提前结束的.

Example:

.. code-block:: python

    break_out = False
    for x in seq:
        do_something(x)
        if condition(x):
            break_out = True
            break
        do_something_else(x)

    if not break_out:
        print("I didn't break out!")


另一种更简单的方法是在循环中添加一个\ ``else``\ 子句, 它仅在没有调用\ ``break``\ 时才执行**\ .

Example:

.. code-block:: python

    from math import sqrt

    for n in range(99, 81, -1):
        root = sqrt(n)
        if root == int(root):
            print(n)
            break
    else:
        print("Didn't find it")

无论是\ ``for``\ 循环还是\ ``while``\ 循环中, 都可使用\ ``continue``, ``break``\ 和\ ``else``\ 子句.

