Python继承机制
==============

继承, 是根据已有的类创建新的类. 
新类继承了已有类的所有属性和方法, 并可在已有类的基础上添加一些成员(属性和方法).
通过使用继承机制, 可以轻松实现类的复用.

在Python中, 实现继承的类称为\ **子类**\ ，被继承的类称为\ **超类**\ (也可称为基类, 父类). 

子类继承超类时, 只需要在定义子类时, 将超类(可以是多个)放在子类之后的圆括号中即可:

.. code-block:: python
    :emphasize-lines: 1

    class 类名(超类1, 超类2, ...):
        ...

*   子类继承超类的所有属性和方法;
*   可以在子类中重写从超类继承的方法.

.. note::

    对于新式类, 如果该类没有显式指定继承自哪个类, 则默认继承\ ``object``\ (``object``\ 类是Python中所有类的超类, 即要么是直接超类, 要么是间接超类). 

    另外, Python支持多重继承, 即一个子类可以同时拥有多个直接超类.


多重继承
--------

大部分面向对象的编程语言都只支持单继承, 即子类只能有一个父类, 而Python支持多重继承(C++也支持多重继承), 即一个子类可以从多个父类继承.
和单继承相比, 多重继承容易让代码逻辑复杂, 思路混乱, 一直备受争议, Java/C#/PHP等干脆取消了多重继承.

**使用多重继承时, 有一点务必注意: 如果多个父类以不同的方式实现了同一个方法(即有多个同名方法), 
必须在class语句中小心排列这些父类, 因为位于前面的类中的方法将覆盖位于后面的类的同名方法.**

.. note::

    多重继承是一个功能强大的工具, 然而, 除非确实有需要, 否则应该避免使用多重继承, 因为在有些情况下, 它可能带来意外的并发症.


方法解析顺序
------------

Python支持(多)继承, 一个类的方法和属性可能定义在当前类, 也可能定义在父类. 
对于这种情况, 当调用类方法或者属性时, 就需要对当前类以及它的基类进行搜索, 以确定方法或属性的位置, 而搜索的顺序就称为\ **方法解析顺序**\ .

方法解析顺序(Method Resolution Order), 简称MRO. 
对于只支持单继承的编程语言来说, MRO很简单, 就是从当前类开始, 逐个搜索它的父类; 
而对于Python, 它支持多重继承, MRO相对会复杂一些.

实际上, Python发展至今, 经历了以下3种MRO算法, 分别是:

    *   从左往右, 采用深度优先搜索(DFS)的算法, 称为旧式类的MRO;
    *   自Python 2.2开始, 新式类在采用深度优先搜索算法的基础上, 对其做了优化;
    *   自Python 2.3开始, 对新式类采用了C3算法. 由于Python 3.x只支持新式类, 所以只支持C3算法.

C3算法可以确保:

    *   **父类不比子类先查找;**
    *   **如果有多个父类, 先查找定义在前面的父类.**

 
重写普通方法和特殊的构造方法
----------------------------

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

例如, 接上面的例子, B可以重写方法\ `hello`\ :

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


使用\ ``super()``\ 函数调用超类方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

对于新式类, 我们应该使用\ ``super()``\ 函数. 
调用该函数时, 将当前类和当前实例作为参数. 
对其返回的对象调用方法时, 调用的将是超类(而不是当前类)的方法.
在Python 3.x中, 在类中调用\ ``super()``\ 函数时, 可以不提供任何参数(通常也应该这么做).

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

