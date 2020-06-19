Python闭包
==========

闭包, 又称闭包函数或者闭合函数, 和嵌套函数类似, 不同之处在于, 闭包中外部函数返回的不是一个具体的值, 而是一个函数. 
一般情况下, 返回的函数会赋值给一个变量, 这个变量可以在后面被继续执行调用.

通常, 在函数中定义的局部变量(包括函数参数), 在函数外部是无法使用的, 而且当函数退出时, 它们会被销毁.
但是在闭包中, 作为外部函数返回值的函数在返回时, 会携带其所在的环境, 即会携带外部函数中定义的局部变量.

Example:

.. code-block:: python
    :emphasize-lines: 2, 5, 7, 13

    def multiplier(factor):
        def multiplierByFactor(number):
            return number * factor

        return multiplierByFactor

    f1 = multiplier(10)
    print(f1(5))
    print(f1(6))

    print('------')

    f2 = multiplier(20)
    print(f2(5))
    print(f2(6))

    运行结果为:
    50
    60
    ------
    100
    120

以上面的程序为例说明:

    ``multiplier()``\ 函数以一个内部函数作为返回值, 其就是一个闭包函数; 
    通常, 当调用\ ``multiplier()``\ 函数结束后, 其中定义的局部变量和参数都会被释放(``factor``\ 参数), 
    但因为\ ``multiplier()``\ 是一个闭包函数, 所以其返回的\ ``multiplerByFactor()``\ 函数携带了\ ``multiplier()``\ 函数的局部变量和参数, 
    在返回的\ ``multiplierByFactor()``\ 函数中仍能使用\ ``multiplier()``\ 函数中定义的局部变量和参数.
   
    在执行\ ``f1 = mulplier(10)``\ 时, 在返回的\ ``f1``\ 对象中, ``factor``\ 被赋值为10, 
    而且虽然\ ``multiplier()``\ 函数已经调用结束, 但\ ``f1``\ 仍能范围\ ``factor``\ 参数.    


Python闭包的\ ``__closure__``\ 属性
-----------------------------------

闭包比普通的函数多了一个\ ``__closure__``\ 属性, 里面定义了一个元组用于存放所有的cell对象, 每个cell对象一一保存了这个闭包中所有的的外部变量.
当闭包被调用时, 系统会根据\ ``__closure__``\ 属性找到对应的自由变量, 完成整体的函数调用.

Example:

.. code-block:: python

    def multiplier(factor):
        def multiplierByFactor(number):
            return number * factor

        return multiplierByFactor

    f1 = multiplier(10)
    print(f1.__closure__[0].cell_contents)

    运行结果为:
    10


为什么要使用闭包?
-----------------

*   使用闭包, 可以让程序更加简洁;
*   如果在函数的开头需要做一些额外工作, 当需要多次调用该函数时, 如果将那些额外工作的代码放在外部函数, 就可以减少调用导致的不必要开销, 提高程序的运行效率.

