重写超类方法
============

每个类都有一个或多个超类, 并从它们那里继承属性和方法.
对子类的实例调用方法(或访问其属性)时, 如果找不到该方法(或属性), 将在其超类中查找.

Example:

.. code-block:: python
    :emphasize-lines: 11

    class A:
        def hello(self):
            print("Hello, I am A")

    class B(A):
        pass

    a = A()
    b = B()
    a.hello()
    b.hello()

    运行结果为:
    Hello, I am A
    Hello, I am A

由于在类B中没有定义方法\ ``hello``\ , 因此对其调用方法\ ``hello``\ 时, 调用的是从类A继承的方法\ ``hello``\ .

**要在子类中添加功能, 一种基本方式是添加方法. 
然而, 有时候我们可能想重写超类的某些方法, 以定制继承而来的行为.**

**重写的方法就是在子类中定义并实现同名的方法. 
这样, 原来从超类中继承的同名方法就被"遮蔽"了, 调用该方法时, 默认调用的是重写后的方法.**

例如, 接上面的例子, B可以重写方法\ ``hello``\ :

.. code-block:: python
    :emphasize-lines: 6

    class A:
        def hello(self):
            print("Hello, I am A")

    class B(A):
        def hello(self):
            print("Hello, I am B")

    a = A()
    b = B()
    a.hello()
    b.hello()

    运行结果为:
    Hello, I am A
    Hello, I am B

.. attention::

    Python并不支持函数的重载, 即使重写后的方法原型有所改变, 原来的同名方法也会别"遮蔽".


**重写是继承机制的一个重要方面, 对构造函数来说尤其如此. 
构造函数用于初始化新建对象的状态,而对大多数子类来说, 除超类的初始化代码之外, 还需要有自己的初始化代码. 
虽然所有方法的重写机制都相同, 但与重写普通方法相比, 重写构造函数时更有可能遇到一个特别问的问题: 重写构造函数时, 必须调用超类的构造函数, 否则可能无法正确地初始化对象.**

Example:

.. code-block:: python

    class Bird:
        def __init__(self):
            self.hungry = True
        def eat(self):
            if self.hungry:
                print('Aaaah...')
                self.hungry = False
            else:
                print('No, thanks!')

    class SongBird(Bird):
        def __init__(self):
            self.sound = 'Squawk!'
        def sing(self):
            print(self.sound)

    sb = SongBird()
    sb.eat()

    运行结果为:
    Traceback (most recent call last):
        File "./a.py", line 21, in <module>
            sb.eat()
        File "./a.py", line 7, in eat
            if self.hungry:
    AttributeError: 'SongBird' object has no attribute 'hungry'

``SongBird``\ 是\ ``Bird``\ 的子类, 继承了方法\ ``eat``\ , 但如果尝试调用它, 将引发异常.

异常清楚地指出了问题出现但原因: ``SongBird``\ 没有属性\ ``hungry``\ . 
为何会这样呢? 因为在\ ``SongBird``\ 中重写了构造函数, 但新的构造函数没有包含任何初始化属性\ ``hungry``\ 的代码. 
要消除这种错误, ``SongBird``\ 的构造函数中必须调用其超类的构造函数, 以确保基本的初始化得以执行. 


调用超类方法
------------

有两种方法来调用超类方法:

    *   调用未关联的超类方法
    *   使用\ ``super()``\ 函数(推荐的方法)

 
调用未关联的超类方法
^^^^^^^^^^^^^^^^^^^^

通过实例调用方法时, 方法的参数\ ``self``\ 将自动关联到实例. 
然而, 如果通过类调用方法, 就没有实例与其相关联. 
在这种情况下, 便可设置参数\ ``self``\ , 这样的方法称为\ **未关联的**\ .

*   在子类的构造函数中调用超类的构造函数

Example:

.. code-block:: python
    :emphasize-lines: 14

    class Bird:
        def __init__(self):
            self.hungry = True
        def eat(self):
            if self.hungry:
                print('Aaaah...')
                self.hungry = False
            else:
                print('No, thanks!')
                
    class SongBird(Bird):
        def __init__(self):
            # 调用超类的构造函数
            Bird.__init__(self)
            self.sound = 'Squawk!'
        def sing(self):
            print(self.sound)

*   在类外调用子类的超类方法

Example:

.. code-block:: python
    :emphasize-lines: 11

    class A():
        def hello(self):
            print("Hello, I am A")

    class B(A):
        def hello(self):
            print("Hello, I am B")

    b = B()
    # 调用超类的方法
    A.hello(b)

    运行结果为:
    Hello, I am A


使用\ :ref:`super-reference-label`\ 函数调用超类方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

*   使用\ ``super()``\ 函数, 调用子类的超类方法

Example:

.. code-block:: python
    :emphasize-lines: 11

    class A():
        def hello(self):
            print("Hello, I am A")

    class B(A):
        def hello(self):
            print("Hello, I am B")

    b = B()
    # 使用super()函数调用超类方法
    super(B, b).hello()

    运行结果为:
    Hello, I am A

*   在类中不带任何参数调用\ ``super()``\ 函数

Exmaple:

.. code-block:: python
    :emphasize-lines: 14

    class Bird:
        def __init__(self):
            self.hungry = True
        def eat(self):
            if self.hungry:
                print('Aaaah...')
                self.hungry = False
            else:
                print('No, thanks!')

    class SongBird(Bird):
        def __init__(self):
            # 使用super()函数, 调用超类的构造函数
            super().__init__()
            self.sound = 'Squawk!'
        def sing(self):
            print(self.sound)


调用超类的构造方法
------------------

子类会继承超类所有的属性和方法, 父类的构造方法, 子类也同样会继承.

Python是一门支持多重继承的面向对象编程语言, 如果子类继承的多个超类中包含同名的方法, 则子类对象在调用该方法时, 会优先选择排在前面的超类中的该方法. 
构造方法也是如此.

有的时候, 我们需要在子类中自定义构造方法, 则必须在该方法中调用超类的构造方法(尤其是多重继承时, 需要调用多个超类的构造方法). 

在子类的构造方法中, 调用超类构造方法的方式有两种(和调用超类的普通方法一样), 分别是:

    *   调用未关联的超类方法;
    *   使用\ ``super()``\ 函数. 但如果涉及多重继承, 该函数只能调用第一个直接父类的构造方法.

.. note::

    如果涉及到多重继承, 在子类的构造方法中, 调用第一个父类的构造方法的方式有以上两种, 而调用其它父类的构造方法的方式只能使用未关联的超类方法.

