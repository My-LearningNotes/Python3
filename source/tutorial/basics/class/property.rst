``property()``\ 函数和\ ``@property``\ 装饰器
=============================================


``property()``\ 函数
--------------------

我们一直使用\ ``对象.属性``\ 的方式访问类中定义的属性, 其实这种做法是欠妥的, 因为它破坏了封装的原则. 
正常情况下, 类包含的属性应该是隐藏的, 只允许通过类提供的方法来间接实现对类属性的访问和操作.

因此, 在不破坏类封装原则的基础上, 为了能够有效操作类中的属性, 类中应包含读/写类属性的多个\ ``getter``\ /``setter``\ 方法, 这样就可以通过\ ``对象.方法``\ 的方式操作属性. 
但是这样的操作比较繁琐, 没有直接使用\ ``对象.属性``\ 方便.

Python中提供了\ ``property()``\ 函数, 可以在不破坏类的封装原则的前提下, 依然可以使用\ ``对象.属性``\ 的方式操作类中的属性.

``property()``\ 函数的基本使用格式如下:

.. code-block:: python

    属性名 = property(fget=None, fset=None, fdel=None, doc=None)

其中, ``fget``\ 参数用于指定获取该属性值的方法, ``fset``\ 参数用于指定设置该属性值的方法, 
``fdel``\ 参数用于指定删除该属性值的方法, 最后的\ ``doc``\ 是该属性的文档字符串.

    *   如果指定了\ ``fget``\ , 则该属性是可读的;
    *   如果指定了\ ``fset``\ , 则该属性是可写的;
    *   如果指定了\ ``fdel``\ , 则该属性是可删除的.

Example:

.. code-block:: python

    class Student:
        # 构造函数
        def __init__(self, name):
            self.__name = name
        # name属性的setter
        def setname(self, name):
            self.__name = name
        # name属性的getter
        def getname(self):
            return self.__name
        # name属性的deleter
        def delname(self):
            self.__name = 'xxx'

    name = property(fget=getname, fset=setname, fdel=delname, doc='hello')

    # 显示属性的文档字符串的两种方式
    print(Student.name.__doc__)
    help(Student.name)

    s1 = Student('sylar')
    # 调用getname()方法
    print(s1.name)
    # 调用setname()方法
    s1.name = 'John'
    print(s1.name)
    # 调用delname()方法
    del s1.name
    print(s1.name)

需要注意的是, 在上面的代码中, ``getname()``\ 方法中需要返回\ ``name``\ 属性, 如果使用\ ``self.name``\ 的话, 其本身又将调用\ ``getname()``\ ，这样会进入无限递归. 
为了避免这种情况, 代码中的\ ``name``\ 属性必须设置为私有属性, 即使用\ ``__name``\ .


.. note::

    在没有为属性设置\ ``property()``\ 函数之前, 对属性读/写/删除, 都是直接对属性的操作. 
    为属性设置了\ ``property()``\ 函数之后, 对属性的读/写/删除, 都是调用\ ``property()``\ 函数中定义的方法.


``@property``\ 装饰器
---------------------

除了使用\ ``property()``\ 函数之外, Python还提供了\ ``@property``\ 装饰器. 
通过\ ``@property``\ 装饰器, 可以定义属性并同时定义该属性的getter方法.

``@property``\ 的语法格式如下:

.. code-block:: python

    @property
    def 属性名(self):
        代码块


*   setter装饰器

使用\ ``@property``\ 装饰器定义一个属性之后, 可以使用\ ``setter``\ 装饰器定义该属性的setter方法.

.. code-block:: python

    @属性名.setter
    def 属性名(self, value):
        代码块


*   deleter装饰器

使用\ ``@property``\ 装饰器定义一个属性之后, 可以使用\ ``deleter``\ 装饰器定义该属性的deleter方法.

.. code-block:: python

    @属性名.deleter
    def 属性名(self):
        代码块

Example:

.. code-block:: python

    class Student:

        def __init__(self, name):
            self.__name = name

        @property
        def name(self):
            return self.__name

        @name.setter
        def name(self, name):
            self.__name = name

        @name.deleter
        def name(self):
            self.__name = 'xxx'

    s1 = Student('sylar')
    # 调用getter方法
    print(s1.name)
    # 调用setter方法
    s1.name = 'John'
    print(s1.name)
    # 调用deleter方法
    del s1.name
    print(s1.name)

