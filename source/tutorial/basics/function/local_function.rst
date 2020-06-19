Python局部函数
==============

Python函数内部可以定义变量, 这样就产生了局部变量. 
Python还支持函数的嵌套定义, 即在函数的内部定义函数, 这样的函数称为\ **局部函数**\ .

*   和局部变量一样, 默认情况下局部函数只能在其所在函数的作用域内使用;

Example:

.. code-block:: python

    # 全局函数
    def foo():
        # 局部函数
        def bar():
            print('Hello, world')
        # 调用局部函数
        bar()
    # 调用全局函数
    foo()

    运行结果为:
    Hello, world

*   就如同全局函数返回其局部变量, 就可以扩大该变量的作用域一样, 通过将局部函数作为所在函数的返回值, 也可以扩大局部函数的作用域;

Example:

.. code-block:: python

    # 全局函数
    def foo():
        # 局部函数
        def bar():
            print('Hello, world')
        # 将局部函数作为返回值
        return bar
    # 调用全局函数
    f = foo()
    # 调用全局函数中的局部函数
    f()

    运行结果为:
    Hello, world


对于局部函数的作用域可以总结为:

    *   如果所在函数没有返回局部函数, 则局部函数的可用范围仅限于所在的函数内部;
    *   如果所在函数将局部函数作为返回值, 则局部函数的作用域就会扩大, 既可以在所在函数内部使用, 也可以在所在函数的作用域中使用.


另外值得一提的是, 如果局部函数中定义有和其所在的函数中变量同名的变量, 也会发生"遮蔽"问题.

Example:

.. code-block:: python

    def foo():
        msg = 'foo'

        def bar():
            print(msg)
            msg = 'bar'

        bar()

    foo()

运行上面的代码，报错:

.. code-block:: text

    UnboundLocalError: local variable 'msg' referenced before assignment

导致该错误的原因是: 

    局部函数\ ``bar()``\ 中定义的\ ``msg``\ 变量"遮蔽"了所在函数\ ``foo()``\ 中定义的\ ``msg``\ 变量. 
    再加上\ ``bar()``\ 函数中\ ``msg``\ 变量的定义在\ ``print(msg)``\ 语句之后, 违反了\ **先定义后使用**\ 的原则, 因此程序报错.

在这里, 由于\ ``foo()``\ 函数定义的\ ``msg``\ 变量也是局部变量, 因此这里不能使用\ ``globals()``\ 函数或者\ ``global``\ 关键字.
这里可以使用Python提供的\ ``nonlocal``\ 关键字, 在局部函数中声明一个变量为外部函数中的变量. 
这样, 所有对该变量的操作都是对外部变量的操作(赋值操作不会定义变量, 而是对外部变量的修改).

.. attention::

    同\ ``global``\ 关键字一样, 使用\ ``nonlocal``\ 修饰的变量不能直接赋值, 需要先声明再赋值.

Example:

.. code-block:: python
    :emphasize-lines: 4

    foo():
        msg = 'foo'
        def bar():
            nonlocal msg
            print(msg)
            msg = 'bar'

        bar()
        print(msg)

    foo()

    运行结果为:
    foo
    bar

