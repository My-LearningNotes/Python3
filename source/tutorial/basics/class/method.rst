Python实例方法, 静态方法和类方法
================================

和类属性一样, 类方法也可以进行更细致的划分, 具体可分为: 类方法, 实例方法和静态方法.
可以如下简单区分这三种方法:

    *   采用\ ``@classmethod``\ 修饰的方法是\ **类方法**\ ;
    *   采用\ ``@staticmethod``\ 修饰的方法是\ **静态方法**\ ;
    *   不用任何修饰的方法是\ **实例方法**\ .


实例方法
--------

通常情况下, 在类中定义的方法默认都是实例方法.

*   实例方法最大的特点就是, 它至少包含一个\ ``self``\ 参数(且必须是第一个参数), 用于绑定调用此方法的实例对象(Python会自动绑定).

*   实例方法通常会通过实例对象来调用, Python也支持通过类名调用实例方法, 但此方式需要手动给\ ``self``\ 参数传值.

.. note::

    通过对象调用实例方法, Python会自动将对象绑定到实例方法的\ ``self``\ 参数, 这种方式称为\ **绑定方法**\ ;

    通过类名来调用实例方法, 需要手动的为实例方法传递\ ``self``\ 参数, 这方方式称为\ **非绑定方法**\ .

Example:

.. code-block:: python

    class Person:
        def __init__(self):
            self.name = 'sylar'
            self.age = 18

        def showInfo(self):
            print(f'name: {self.name}, age: {self.age}')

    A = Person()
    # 通过实例对象调用实例方法
    A.showInfo()

    # 通过类名调用实例方法
    Person.showInfo(A)

    运行结果为:
    sylar 18
    sylar 18

    
类方法
------

Python类方法和实例方法类似, 它至少也要包含一个参数, 只不过类方法中通常将其命名为\ ``cls``\ , 
Python会自动将类本身绑定给\ ``cls``\ 参数(注意, 绑定的不是实例对象). 
也就是说, 我们在调用类方法时, 无需显式为\ ``cls``\ 传递参数.

.. note::

    和\ ``self``\ 一样, ``cls``\ 参数的命名也不是规定的(可以随意命名), 只是Python程序员约定俗成的习惯而已.

*   和实例方法最大的不同在于, 类方法需要使用\ ``@classmethod``\ 修饰符进行修饰(如果没有使用修饰符, 则Python将其作为实例方法, 而不是类方法);

*   类方法推荐通过类名来调用, 也可以通过实例对象来调用(不推荐);

*   类方法只能访问类属性, 不能访问对象属性(因为类方法的\ ``cls``\ 参数绑定的是类本身).

Example:

.. code-block:: python

    class Person:
        # 定义类属性
        name = 'hello'
        age = 100

        # 定义实例方法
        def __init__(self):
            # 定义实例属性
            self.name = 'sylar'
            self.age = 18

        # 定义类方法
        @classmethod
        def showInfo(cls):
            print(f'name: {cls.name}, age: {cls.age}')

    A = Person()
    # 通过对象调用类方法(不推荐)
    A.showInfo()

    # 通过类调用类方法(推荐)
    Person.showInfo()


    运行结果:
    name: hello, age: 100
    name: hello, age: 100
    # 因为调用的是类方法, cls参数绑定的是类本身


静态方法
--------

静态方法其实就是函数, 不过是定义在类命名空间中, 而函数是定义全局命名空间中.

*   静态方法没有类似\ ``self``\, ``cls``\ 这样的特殊参数, 因此Python解释器不会对它包含的参数做任何类或对象的绑定,
    也正因为如此, 在静态方法中无法调用任何类属性和类方法;

*   静态方法需要使用\ ``@staticmethod``\ 修饰;

*   静态方法的调用, 既可以使用类名, 也可以使用对象.

Example:

.. code-block:: python

    class Person:
        # 定义静态方法
        @staticmethod
        def showInfo(name, age):
            print(name, age)

    # 通过类名调用静态方法
    Person.showInfo('sylar', 18)

    A = Person()
    # 通过对象调用静态方法
    A.showInfo('Jack', 20)

    运行结果为:
    sylar 18
    Jack 20

在实际编程中, 几乎不会用到类方法和静态方法, 因为完全可以使用函数替代它们想要实现的功能, 
但在一些特殊的场景中(例如工厂模式中), 使用类方法和静态方法也是很不错的选择.

