Python装饰器
============

**装饰器本质上是一个Python函数或类, 它可以让其它函数或类在不需要做任何代码修改的前提下扩展功能, 装饰器的返回值也是一个函数/类对象.**

它经常用于有下面需求的场景, 比如: 插入日志, 性能测试, 事物处理, 缓存, 权限校验等场景, 装饰器是解决这类问题的绝佳设计.
有了装饰器, 我们就可以抽离出大量与函数功能本身无关的相同代码到装饰器中并重用.
**概括的讲, 装饰器的作用就是为已经存在的对象添加额外的功能.**

例如, 定义一个函数\ ``foo``\ :

.. code-block:: python

    def foo():
        print('i am foo')

现在有一个新的需求, 希望可以记录下函数的执行日志, 最直接的方法就是侵入代码里面修改:

.. code-block:: python

    def foo():
        print('i am foo')
        logging.info('foo is running')

如果函数\ ``bar()``\ , ``bar2()``\ 也有类似的需求, 怎么做呢? 
再在\ ``bar()``\ 函数里写一个\ ``logging``\ ? 这样就造成大量雷同的代码. 
为了减少重复代码, 我们可以这样做: **重新定义一个新的函数**\ , 专门处理日志, 日志处理完之后再执行真正的业务逻辑.

.. note::

    在Python中, 函数可以赋值给变量, 可以作为参数传递给函数, 可以作为函数的返回值.

.. code-block:: python
    :emphasize-lines: 1

    def use_logging(func):
        logging.warn('%s is running' % func.__name__)
        func()

    def foo():
        print('i am foo'0

    use_logging(foo)


这样做逻辑上是没问题的, 功能是实现了, 但是我们调用的时候不再是调用真正的业务逻辑函数\ ``foo()``\ , 而是换成了\ ``use_logging``\ 函数, 这样就破坏了原有的代码结构. 

那么有没有更好的方式呢? 当然有, 答案就是装饰器.


简单装饰器
----------

.. code-block:: python
    :emphasize-lines: 3, 4, 5, 6, 7, 8

    import logging

    def use_logging(func):
        def wrapper():
            logging.warn('%s is running' % func.__name__)
            return func()

        return wrapper

    def foo():
        print('i am foo')


    foo = use_logging(foo)
    foo()

``use_logging``\ 就是一个装饰器, 它是一个普通的函数, 它把真正执行业务逻辑的函数\ ``func``\ 包裹在其中, 看起来像\ ``foo``\ 被\ ``use_logging``\ 装饰了一样, 
``use_logging``\ 返回的也是一个函数, 这个函数的名称叫\ ``wrapper``\ .

.. note::

    **装饰器就是一个函数, 它以一个函数为参数, 返回一个新的函数, 从而实现对原函数功能上的扩展.**


``@``\ 语法糖
-------------

``@``\ 符号就是装饰器的语法糖, 它放在函数开始定义的地方.

.. code-block:: python
    :emphasize-lines: 10

    import logging

    def use_logging(func):
        def wrapper():
            logging.warn('%s is running' % func.__name__)
            return func()

        return wrapper

    @use_logging
    def foo():
        print('i am foo')


    foo()

如上所示, 有了\ ``@``\ , 就可以省去\ ``foo = use_logging(foo)``\ 这条语句了, 直接调用\ ``foo()``\ 即可得到想要的结果. ``foo()``\ 函数不需要做任何修改, 只需要在定义的地方加上装饰器, 调用的时候还是和以前一样.
如果我们有其它的类似函数, 我们可以继续调用装饰器来修饰函数, 而不用重复修改函数或增加新的封装. 
这样, 我们就提高了代码的复用性和可读性. 

装饰器在Python中使用如此方便都要归因于Python的函数能像普通的变量一样作为参数传递给其它函数, 可以赋值给变量, 可以作为函数的返回值, 可以被定义在另一个函数内.

.. note::

    ``@``\ **是Python装饰器的语法糖, 在函数定义时使用**\ ``@装饰器``\ **修饰的函数, 在调用该函数时, 执行的是装饰器函数返回的那个函数.**


带参数的装饰器
--------------

如果业务逻辑函数\ ``foo``\ 需要参数怎么办呢?


带有固定参数的装饰器
^^^^^^^^^^^^^^^^^^^^

当业务逻辑函数\ ``foo``\ 带有固定的参数时, 可以在装饰器中定义\ ``wrapper``\ 函数时定义相同的函数原型:

.. code-block:: python
    :emphasize-lines: 4, 11

    import logging

    def use_logging(func):
        def wrapper(name, age):
            logging.warn('%s is running' % func.__name__)
            return func(name, age)

        return wrapper

    @use_logging
    def foo(name, age):
        print(f'i am {name}, age is {age}')

    foo('sylar', 18)


无固定参数但装饰器
^^^^^^^^^^^^^^^^^^

当装饰器不知道业务逻辑函数\ ``foo``\ 到底有多少个参数时, 可以使用\ ``*args``\ 来表示位置参数, 使用\ ``**kwargs``\ 来表示关键字参数:

.. code-block:: python
    :emphasize-lines: 4

    import logging

    def use_logging(func):
        def wrapper(*args, **kwargs):
            logging.warn('%s is running' % func.__name__)
            return func(*args, **kwargs)

        return wrapper

    @use_logging
    def foo(name, age):
        print(f'i am {name}, age is {age}')

    foo('sylar', 18)


装饰器调用顺序
--------------

一个函数可以同时定义多个装饰器, 比如:

.. code-block:: python
    :emphasize-lines: 1, 2, 3

    @a
    @b
    @c
    def foo():
        pass

它的执行顺序是\ **从里到外的, 最先调用最里层的装饰器, 最后调用最外层的装饰器**\ , 它等效于:

.. code-block:: python

    foo = a(b(c(foo)))


类装饰器
--------

装饰器不仅可以是函数, 还可以是类, 相比函数装饰器, 类装饰器具有灵活度大, 高内聚, 封装性等优点. 

使用类装饰器主要依靠类的\ ``__call__()``\ 方法, 当使用\ ``@``\ 将类装饰器附加到函数上时, 就会调用此方法.

Example:

.. code-block:: python

    class Foo:
        def __init__(self, func):
            self._func = func

        def __call__(self):
            def wrapper():
                print('class decorator running')
                self._func()
                print('class decorator ending')

            return wrapper


Python内置装饰器
----------------

在Python中有三个内置的装饰器, 都是跟class相关的: ``staticmethod``, ``classmethod``\ 和\ ``property``\ .

    * ``staticmethod``\ 是类静态方法, 其跟成员方法的区别是没有\ ``self``\ 参数, 并且可以在类不进行实例化的情况下调用;
    * ``classmethod``\ 与成员方法的区别在于所接收的第一个参数不是\ ``self``\ (类实例指针), 而是\ ``cls``\ (当前类的具体类型);
    * ``property``\ 是属性的意思, 表示可以通过类实例直接访问的信息.

