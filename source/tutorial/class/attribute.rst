Python类属性和实例属性
======================

无论是类属性还是类方法, 都无法像普通变量或函数那样, 在类的外部直接使用它们.
我们可以将类看做一个独立的空间, 则类属性其实就是在类体中定义的变量, 类方法就是在类体中定义的函数.

在类体中, 根据变量定义的位置不同, 以及定义的方式不同, 类属性又可细分为以下三种类型:

    *   在类体中, 所有函数之外: 此范围定义的变量, 称为\ **类属性**\ 或\ **类变量**\ ;
    *   在类体中, 函数内部: 以\ ``self.var = value``\ 的方式定义的变量, 称为\ **实例属性**\ 或\ **实例变量**\ ;
    *   在类体中, 函数内部: 以\ ``var = value``\ 的方式定义的变量, 称为\ **局部变量**\ .


类属性(类变量)
--------------

类属性指的是在类中, 但在各个类方法外定义的变量.
类属性的特点是, 所有类的实例化对象都共享类属性, 也就是说, 类属性在所有实例化对象中是作为公用资源存在的.

类属性的调用方式有两种, 既可以使用类名调用, 也可以使用类的实例化对象调用(不推荐).

Example:

.. code-block:: python

    class Person:
        # 定义类属性
        name = 'sylar'
        age = 18

    # 通过类名访问类属性
    print(Person.name, Person.age)

    A = Person()
    # 通过实例对象访问类属性
    print(A.name, A.age)

因为类属性是所有实例对象共享的, 通过类名修改类属性, 会影响该类的所有对象.

除了可以通过类名访问类属性之外, 还可以动态地为类和对象添加类属性.

Example:

.. code-block:: python

    class Person:
        # 定义类属性
        name = 'sylar'
        age = 18

    Person.sex = 'male'

    A = Person()
    print(A.sex)


实例属性(实例变量)
------------------

实例属性指的是在任意类方法内部, 以\ ``self.var``\ 的方式定义的变量, 实例属性是每个对象独有的.
另外, 实例属性只能通过对象名访问, 无法通过类名访问.

Example:

.. code-block:: python

    class Person():
        def __init__(self):
            # 定义实例属性
            self.name = 'sylar'
            self.age = 18

        def showInfo(self):
            print(f'name: {self.name}, age: {self.age}')

    A = Person()
    A.showInfo()

**通过实例对象可以方位类属性, 但无法修改类属性.** 
这是因为, 通过实例对象的赋值操作, 不是在给"修改类属性", 而是在定义新的实例属性.

Example:

.. code-block:: python

    class Person():
        # 定义类属性
        name = 'sylar'
        age = 18

    A = Person()
    # 定义新的实例属性
    A.name = 'Jack'
    A.age = 20
    # 打印实例属性
    print(A.name, A.age)
    # 打印类属性
    print(Person.name, Person.age)

.. note::

    在类中, 实例属性和类属性可以同名, 但这种情况下使用实例对象无法调用类属性, 它会首先实例属性, 所以\ **不推荐通过实例对象调用类属性**\ .


局部变量
--------

在类方法中, 直接以\ ``var = value``\ 方式定义的变量属于局部变量.

通常情况下, 定义局部变量是为了所在类方法功能的实现. 
需要注意的一点是, 局部变量只能用于所在函数, 函数执行完成后, 局部变量也会被销毁.

Example:

.. code-block:: python

    class Person():
        def showInfo(self):
            # 在类方法中定义局部变量
            msg = 'hello, world'
            print(msg)

    A = Person()
    A.showInfo()

