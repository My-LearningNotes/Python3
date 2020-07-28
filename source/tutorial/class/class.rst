Python类
========

Python程序中类的使用顺序是这样的:

    *   定义类
    *   创建类的实例(对象)


定义类
------

在Python中, 使用关键字\ ``class``\ 定义类, 基本的语法格式如下:

.. code-block:: python

    class 类名:
        多个(>=0)类属性...
        多个(>=0)类方法...

无论是类属性还是类方法, 对于类来说, 它们都不是必需的, 可以有也可以没有. 
另外, Python类中属性和方法所在的位置也是任意的, 即它们之间并没有固定的前后次序.

类属性就是类中的变量, 类方法就是类中的函数. 
换句话说, 类属性和类方法其实分别是包含在类中的变量和函数的别称. 
需要注意的一点是, 同属一个类的所有类属性和类方法, 要保持统一的缩进格式(通常是4空格缩进).

Python类是由类头(``class 类名``)和类体(统一缩进的变量和函数)构成.
和函数一样, 也可以为类定义\ **文档字符串**\ ，其要放在类头之后, 类体之前的位置.


``__init__()``\ 方法
--------------------

在创建类时, 可以手动添加一个\ ``__init__()``\ 方法, 该方法是一个特殊的类方法, 称为\ **构造方法**\ （或\ **构造函数**\ ).

构造方法用于创建对象时使用, 每当创建一个对象时, Python解释器都会自动调用它.
在Python类中, 定义构造方法的语法格式如下:

.. code-block:: python

    def __init__(self, ...):
        code-block

``__init__()``\ 方法可以包含多个参数, 但必须包含一个名为\ ``self``\ 的参数, 且必须作为第一个参数.
如果没有定义构造方法, Python会自动为类添加一个仅包含\ ``self``\ 参数的空的构造方法, 作为默认构造方法.

.. note::

    在Python中, 以双下划线开头和结尾的方法, 都具有特殊意义.


创建对象
--------

创建对象的过程, 又称为类的实例化.

对已定义号的类进行实例化, 其语法格式如下:

.. code-block:: python
    :emphasize-lines: 1

    类名(参数)

定义类时, 如果没有手动添加\ ``__init__()``\ 构造方法, 又或者添加的\ ``__init__()``\ 中仅有一个\ ``self``\ 参数, 则创建类对象时的参数可以省略不写.


对象的使用
----------

总的来说, 实例化后的类对象可以执行以下操作:

    *   访问或修改类对象具有的实例变量, 甚至可以添加新的实例变量或删除已有的实例变量;
    *   调用类对象的方法, 包括调用现有的方法, 以及给类对象动态添加方法.

*   **类对象访问变量或方法**

.. code-block:: python

    对象名.变量名
    对象名.方法名(参数)

*   **给类对象动态添加/删除变量**

Python支持为已创建好的对象动态增加实例变量, 方法也很简单:

.. code-block:: python

    # 赋值即变量定义
    对象名.变量名 = value

也可以动态地删除实例变量:

.. code-block:: python

    del 对象名.变量名

*   **给类对象动态添加方法**

Python也允许为对象动态增加方法.

但是需要注意的是, 对于动态增加的方法, Python不会自动将调用者绑定到第一个参数(即使第一个参数命名为\ ``self``\ 也没用).
在调用动态添加的方法时, 需要手动传递第一个参数.

Example:

.. code-block:: python
    :emphasize-lines: 4, 9, 11
    
    class Student:
        pass

    def SetName(self, name):
        self.name = name

    student = Student()
    # 动态的为对象添加一个方法
    student.SetName = SetName
    # 调用动态添加的方法时, 需要手动传递一个参数
    student.SetName(student, 'hello')

    print(student.name)

如果不想手动传递参数, 可以借助\ ``types``\ 模块中的\ ``MethodType``\ 实现.

Example:

.. code-block:: python
    :emphasize-lines: 4, 11, 13

    class Student:
        pass

    def SetName(self, name):
        self.name = name

    from types import MethodType

    student = Student()
    # 使用types模块中的MethodType为对象动态添加一个方法
    student.SetName = MethodType(SetName, student)
    # 调用动态增加的方法时不用手动传递self
    student.SetName('hello')
    print(student.name)

当然, 如果动态增加的方法中没有使用\ ``self``\ 参数, 可以在调用时不传递对象本身.

Example:

.. code-block:: python
    :emphasize-lines: 4, 8

    class Student:
        pass

    def Show():
        print('hello, world')

    student = Student()
    student.Show = Show
    student.Show()


``self``\ 参数
--------------

在定义类的过程中, 无论是显式创建类的构造方法, 还是向类中添加实例方法, 都要求将\ ``self``\ 参数作为方法的第一个参数.

事实上, Python只是规定, 无论是构造方法还是实例方法, 最少要包含一个参数, 并没有规定该参数的具体名称.
之所以将其命令为\ ``self``\ , 只是程序员之间约定成俗的一种习惯, 遵守这个约定, 可以使我们的代码具有更好的可读性(大家一看到\ ``self``\ , 就知道它的作用).

.. note::

    Python类方法中的\ ``self``\ 参数就相当于C++中的\ ``this``\ 指针.

``self``\ 参数的具体作用是什么呢?

同一个类可以产生多个对象, 当某个对象调用类方法时, 该对象会把自身的引用作为第一个参数自动传给该方法.
换句话说, Python会自动绑定类方法的第一个参数指向调用该方法的对象.
这样, Python解释器就能知道到底要操作哪个对象的方法了.

