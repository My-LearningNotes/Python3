Python枚举类
============


什么是枚举
----------

枚举, 其实就是一种数据类型, 跟\ ``int``\ , ``char``\ 差不多.

枚举类型, 它的值是一个\ **命名的整型常数的集合**\ .


Python枚举类
------------

枚举类, 也是一个类, 它的作用是定义一个命名的整型常数的集合.

从Python 3.4开始, 增加了对枚举类的支持. 
要定义一个枚举类, 只需要令其继承\ ``enum``\ 模块中的\ ``Enum``\ 类即可.

枚举类的成员是一个字典结构, 每个成员都由2部分组成, 分别为\ ``name``\ 和\ ``value``\ , 
其中, ``name``\ 为该枚举变量的名称, ``value``\ 为枚举值.

和普通类的用法不同, 枚举类不能用来实例化对象, 但可以访问枚举类中的成员. 
访问枚举类的成员有多种方法.

Example:

.. code-block:: python
    :emphasize-lines: 1, 3

    from enum import Enum

    class Color(Enum):
        red = 1 #枚举类的成员必须显式定义其值
        green = 2
        blue = 3

    # 调用枚举类的成员的三种方式
    print(Color.red)
    print(Color['red'])
    print(Color(1))

    # 调用枚举类成员的name和value
    print(Color.red.name)
    print(Color.red.value)

    # 遍历枚举类的所有成员
    for color in Color:
        print(color)


枚举类的成员之间不能比较大小, 但可以用\ ``==``\ 或者\ ``is``\ 比较是否相等.

Example:

.. code-block:: python

    print(Color.red == Color.green)
    print(Color.red.name is Color.green.name)

需要注意, 枚举类中各个成员的值, 不能在类的外部做任何修改.

因为Python枚举类的成员是一个字典结构, 所以各个成员的\ ``name``\ 不能相同, 但\ ``value``\ 可以相同. 
在实际的编程中, 如果想避免各个成员的\ ``value``\ 出现相同的情况, 可以借助\ ``@unique``\ 装饰器, 这样当枚举类中出现相同值的成员时, 会引发\ ``ValueError``\ 异常.

Example:

.. code-block:: python
    :emphasize-lines: 3

    from enum import Enum, unique

    @unique
    class Color(Enum):
        red = 1
        gree = 2
        blue = 1
    
    for color in Color:
        print(color.name, color.value)

    运行时报错:
    Traceback (most recent call last):
        File "./my_enum.py", line 6, in <module>
            class Color(Enum):
        File "/usr/lib/python3.6/enum.py", line 836, in unique
            (enumeration, alias_details))
    ValueError: duplicate values found in <enum 'Color'>: blue -> red

