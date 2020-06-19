Python函数高级用法
==================

在Python中, 函数支持赋值, 作为其它函数的参数以及作为其它函数的返回值.

*   **Python允许直接将函数赋值给其它变量**\ ;

这样做的效果是, 程序中也可以用其它变量来调用该函数.

Example:

.. code-block:: python
    :emphasize-lines: 5, 7

    def foo():
        print('hello, world')

    # 将函数赋值给一个变量
    bar = foo
    # 通过该变量来调用函数
    bar()

    运行结果为:
    hello, world


*   **Python支持函数以参数的形式传入其它函数**\ ;

Example:

.. code-block:: python
    :emphasize-lines: 7, 8, 11, 13

    def add(x, y):
        return x + y

    def multi(x, y):
        return x * y

    def f(x, y, op):
        return op(x, y)

    # 求两个数的和
    print(f(2, 3, add))
    # 求两个数的积
    print(f(2, 3, multi))

    运行结果为:
    5
    6

通过将函数作为参数, 可以在调用函数时动态传入函数, 从而实现动态地改变函数中的部分实现代码, 在不同场景中赋予函数不同的作用.


*   **Python支持将函数的返回值也是函数**\ .

Example:

.. code-block:: python
    :emphasize-lines: 3, 6

    def foo():
        # 局部函数
        def bar():
            print('hello, world')
        # 将局部函数作为返回值
        return bar

    f = foo()
    f()

    运行结果为:
    hello, world
