``__repr__()``\ 方法
====================

通常情况下, 直接输出某个实例化对象, 会得到\ ``类型 + object at + 内存地址``\ 这样的信息. 

Example:

.. code-block:: python
    :emphasize-lines: 5, 8

    class Foo:
        pass

    foo = Foo()
    print(foo)

    # 输出结果
    <__main__.Foo object at 0x7ff43a8d7668>


其实, 直接输出某个实例化对象时, 就是调用该对象的\ ``__repr__()``\ 方法, 输出的是该方法的返回值.

.. note::

    Python中的每个类都包含\ ``__repr__()``\ 方法, 因为\ ``object``\ 类包含\ ``__repr__()``\ 方法, 而Python中所有的类都直接或间接继承自\ ``object``\ 类.

默认情况下, ``__repr__()``\ 会返回和调用者相关的\ ``类名 + object at + 内存地址``\ 信息. 
当然, 我们还可以重写这个方法, 从而实现输出实例化对象时, 输出我们想要的信息.

Example:

.. code-block:: python
    :emphasize-lines: 7

    class Foo:
        def __init__(self):
            self.name = 'sylar'
            self.age = 18

        # 重写__repr__()方法
        def __repr__(self):
            return 'Foo[name=' + str(self.name) + ', age=' + str(self.age) + ']'

    foo = Foo()
    print(foo)

    # 运行结果:
    Foo[name=sylar, age=18]

