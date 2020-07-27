生成器
======

**生成器是一种使用普通函数语法定义的迭代器.**


创建生成器
----------

生成器创建起来与函数一样简单.

包含\ ``yield``\ 语句的函数都被称为\ **生成器**\ . 
这不仅仅是名称上的差别, 生成器的行为与普通函数截然不同. 
差别在于, 生成器不是使用\ ``return``\ 返回一个值, 而是可以生成多个值, 每次一个. 
每次使用\ ``yield``\ 生成一个值后, 函数都将冻结, 即在此停止执行, 等待被重新唤醒. 
被重新唤醒后, 函数将从停止的地方开始继续执行.

例如, 定义一个生成某个范围内浮点数的生成器:

.. code-block:: python
    :emphasize-lines: 4

    def frange(start, stop, step):
        x = start
        while x < stop:
            yield x
            x += step

    >>> for n in frange(0, 4, 0.5):
    ...     print(n)
    ...
    0
    0.5
    1.0
    1.5
    2.0
    2.5
    3.0
    3.5

和普通函数不同的是, 生成器只能用于迭代操作. 
下面是一个实验, 展示生成器函数底层的工作机制:

.. code-block:: python

    >>> def countdown(n):
    ...     print('Starting to count from', n)
    ...     while n > 0:
    ...         yield n
    ...         n -= 1
    ...     print('Done!')
    ...

    >>> # Create the generator, notice no output appears
    >>> c = countdown(3)
    >>> c
    <generator object countdown at 0x1006a0af0>

    >>> # Run to first yield and emit a value
    >>> next(c)
    Starting to count from 3
    3

    >>> # Run to the next yield
    >>> next(c)
    2

    >>> # Run to next yield
    >>> next(c)
    1

    >>> # Run to next yield (iteration stops)
    >>> next(c)
    Done!
    Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
    StopIteration
    >>>

一个生成器函数的主要特征是它只回应在迭代中使用到的\ ``next``\ 操作. 
一旦生成器函数返回退出, 迭代终止. 
我们在迭代中通常使用的\ ``for``\语句会自动处理这些细节, 所以你无需担心.

生成器是包含关键字\ ``yield``\ 的函数, 但被调用时不会执行函数体内的代码, 而是返回一个迭代器. 
每次请求值时, 都将执行生成器的代码, 直到遇到\ ``yield``\ 或\ ``return``\ . 
``yield``\ 意味着应生成一个值, 而\ ``return``\ 意味着生成器应停止执行(即不再生成值; 仅当在生成器调用\ ``return``\ 时, 才能不提供任何参数).

换而言之, 生成器由两个单独的部分组成: **生成器函数**\ 和\ **生成器的迭代器**\ . 
生成器的函数是由\ ``def``\ 语句定义的, 其中包含\ ``yield``\ . 
生成器的迭代器是这个函数返回的结果.

Example:

.. code-block::
    :emphasize-lines: 4, 6

    >>> def simple_generator():
    ...     yield 1
    ... 
    >>> simple_generator # 生成器函数
    <function simple_generator at 0x7fcc1f615bf8>
    >>> simple_generator() # 生成器的迭代器
    <generator object simple_generator at 0x7fcc1f61cc50>

对于生成器的函数返回的迭代器, 可以像使用其它迭代器一样使用它.


生成器的方法
------------

* ``send()``

* ``throw()``

用于在生成器中(``yield``\ 表达式处, 即挂起时)引发异常, 调用时可提供一个异常类型, 一个可选值和一个\ ``traceback``\ 对象. 
之后程序会继续执行生成器函数中后续的代码, 直到遇到下一个\ ``yield``\ 语句. 
如果剩余代码执行完毕也没有遇到下一个\ ``yield``\ 语句, 则程序会引发\ ``StopIteration``\ 异常.

Example:

.. code-block:: python
    :emphasize-lines: 9

    def foo():
        try:
            yield 1
        except ValueError:
            print('Capture ValueError')

    f = foo()
    print(next(f))
    f.throw(ValueError)

    # 运行结果:
    1
    Capture exception
    Traceback (most recent call last):
    File "/tmp/snow/generator.py", line 9, in <module>
        f.throw(ValueError)
    StopIteration

一开始执行生成器函数在\ ``yield 1``\ 处挂起, 当执行\ ``throw()``\ 方法时, 它会先抛出\ ``ValueError``\ 异常, 然后继续执行后续代码直到下一个\ ``yield``\ 语句, 
该程序后续代码中不再有\ ``yield``\语句, 因此程序执行到最后, 抛出\ ``StopIteration``\ 异常.

* ``close()``

用于停止生成器, 调用时无需提供任何参数.

方法\ ``close``\ 也是基于异常的: 在\ ``yield``\ 处引发\ ``GeneratorExit``\ 异常. 
因此, 如果要在生成器中提供一些清理代码, 可将\ ``yield``\ 放在一条\ ``try/finally``\ 语句中.
如果愿意, 也可捕获\ ``GeneratorExit``\ 异常, 但随后必须重新引发它(可能在清理后), 引发其它异常或直接返回. 
对生成器调用\ ``close``\ 后, 再试图从它那里获取值将导致\ ``RuntimeError``\ 异常.


生成器推导式
^^^^^^^^^^^^

生成器推导式, 其工作原理和列表推导式相同, 但不是创建一个列表(即不立即执行), 而是返回一个生成器, 让你能够逐步进行计算.

Example:

.. code-block:: python
    :emphasize-lines: 1

    >>> g = ((i + 2) ** 2 for i in range(2, 27)) 
    >>> next(g)
    16
    >>> next(g)
    25

可以看到, 不同于列表推导式, 生成器推导式的定义使用的是圆括号.

直接在一对既有的圆括号内(如在函数调用中)使用生成器推导式时, 无需再添加一对圆括号. 
换而言之, 可编写下面这样非常漂亮的代码:

.. code-block:: python

    sum(i ** 2 for i in range(1))

