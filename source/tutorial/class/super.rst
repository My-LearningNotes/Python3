.. _super-reference-label:

``super()``\ 函数
=================


:ref:`方法解析顺序(MRO) <mro-reference-label>`
----------------------------------------------

对于一个对象, 可以用它的\ ``__mro__``\ 属性或者类的\ ``mro()``\ 函数查看MRO.

调用一个类的方法或访问其属性时, MRO决定了方法/属性的查找顺序. 


``super()``\ 函数的原理
-----------------------

``super()``\ 的工作原理如下:

.. code-block:: python

    super(cls, instance):
        mro = instance.__class__.mro()
        return mro[mro.index(cls) + 1]

其中, ``cls``\ 代表类, ``instance``\ 代表实例, 上面的代码做了两件事:

    * 获取\ ``instance``\ 的MRO列表;
    * 查找\ ``cls``\ 在当前MRO列表中的index, 并返回它的下一个类, 即\ ``mro[index + 1]``\ .

当使用\ ``super(cls, instance)``\ 时, Python会在\ ``instance``\ 的MRO列表中查找\ ``cls``\ 的下一个类.


使用\ ``super()``\ 函数调用超类的方法
-------------------------------------

使用\ ``super()``\ 函数, 可以调用子类的超类方法, 使用时, 将当前的类和对象分别作为参数传入. 
在Python 3.x中, 在类中调用\ ``super()``\ 函数时, 可以不提供任何参数(隐式传入当前的类和\ ``self``\ 参数)来调用其超类方法.

尤其是在重写子类的构造函数时, 可以使用\ ``super()``\ 函数调用超类的构造函数. 
但是, 在使用\ ``super()``\ 函数调用超类的构造函数, 尤其是在多重继承时, 需要特别注意一些问题.

* **不要混用super()函数与显式类调用**

在子类的构造函数中调用超类的构造函数有两种方法: ``super()``\ 函数和显式的类调用. 
这两种方法都可以, 但是\ **不要混用它们**\ . 

.. note::

    有时这种层次结构的一部分位于第三方代码中, 我们无法确定外部包的这些代码中是否使用了\ ``super()``\ , 因此, 当需要对某个第三方类进行子类化时, 最好查看其内部代码以及MRO中其它类的内部代码.

Example:

.. code-block:: python
    :emphasize-lines: 4, 9, 14, 15

    class A():
        def __init__(self):
            print('A', end=' ')
            super().__init__()

    class B():
        def __init__(self):
            print('B', end=' ')
            super().__init__()

    class C(A, B):
        def __init__(self):
            print('C', end=' ')
            A.__init__(self)
            B.__init__(self)

    print('MRO: ', [x.__name__ for x in C.__mro__])
    C()

    运行结果为:
    MRO:  ['C', 'A', 'B', 'object']
    C A B B

可以看到, 上面的代码中, B的构造函数被执行了两次.

在C的构造函数中以显式调用的方式调用了A和B的构造函数, 在A和B的构造函数中使用了\ ``super()``\ 函数调用其父类的构造函数.
结合\ ``MRO``\ 列表和\ ``super()``\ 函数的原理, 在A的构造函数中调用\ ``super().__init__()``\ (``super(A, self).__init__()``), 会使其调用\ ``B.__init__()``\ 方法, 所以导致B的构造函数被调用了两次. 
根本原因还是混用了\ ``super()``\ 函数了显式函数调用.


* 如果类的层次结构中都是使用的\ ``super()``\ 函数, 那么在子类的构造函数中只要调用一个\ ``super().__init__()``\, 就可以完成所有直接和间接父类的初始化.

Example:

.. code-block:: python
    :emphasize-lines: 8, 13, 18

    class Base:
        def __init__(self):
            print('Base', end=' ')

    class A(Base):
        def __init__(self):
            print('A', end=' ')
            super().__init__()

    class B(Base):
        def __init__(self):
            print('B', end=' ')
            super().__init__()

    class C(A, B):
        def __init__(self):
            print('C', end=' ')
            super().__init__()

    print('MRO: ', [x.__name__ for x in C.__mro__])
    C()

    运行结果为:
    MRO:  ['C', 'A', 'B', 'Base', 'object']
    C A B Base


总结
----

* 尽量避免使用多重继承;
* ``super()``\ 的使用必须一致, 即在类的层次结构中, 要么全部使用\ ``super()``\ , 要么全不用, 混用\ ``super()``\ 和传统调用是一种混乱的写法;
* 如果代码要兼容Python 2.x, 在Python 3.x中应该显式继承自\ ``object``\ , 在Python 2.x中没有指定任何祖先的类都被视为旧式类;
* 调用父类时应该提前查看类的层次结构, 也就是使用类的\ ``__mro__``\ 属性或者\ ``mro()``\ 方法查看有关类的MRO.

