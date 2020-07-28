``__dict__``\ 属性
==================

在Python类的内部, 无论是类属性还是实例属性, 都是以字典的形式进行存储的, 其中属性名作为键, 而值作为该键对应的值.

为了方便查看类中包含哪些属性, Python类提供了\ ``__dict__``\ 属性. 
需要注意的一点是, 该属性可以用类名或者类的实例对象来调用, 用类名调用\ ``__dict``\ , 会输出由该类中所有类属性组成的字典; 
而使用类的实例对象调用\ ``__dict``\ , 会输出有类中所有实例属性组成的字典.

Example:

.. code-block:: python
    :emphasize-lines: 10, 14

    class Foo:
        a = 1
        b = 2

        def __init__(self):
            self.name = 'sylar'
            self.age = 18

        #通过类名调用__dict__
        print(Foo.__dict__)

        #通过类实例对象调用__dict__
        foo = Foo()
        print(foo.__dict__)


    # 运行结果:
    {'__module__': '__main__', 'a': 1, 'b': 2, '__init__': <function Foo.__init__ at 0x7f3d8e6ae620>, '__dict__': <attribute '__dict__' of 'Foo' objects>, '__weakref__': <attribute '__weakref__' of 'Foo' objects     >, '__doc__': None}
    {'name': 'sylar', 'age': 18}


对于具有继承关系的父类和子类来说, 父类有自己的\ ``__dict__``\ , 子类也有自己的\ ``__dict__``\ , 且子类不会包含父类的\ ``__dict__``\ .

除此之外, 借助实例对象调用\ ``__dict__``\ 属性获取的字典, 可以使用字典的方式对其中的实例属性的值进行修改. 

Example:

.. code-block:: python
    :emphasize-lines: 13

    class Foo:
        a = 1
        b = 2

        def __init__(self):
            self.name = 'sylar'
            self.age = 18

        #通过类实例对象调用__dict__
        foo = Foo()
        print(foo.__dict__)

        foo.__dict__['age'] = 28
        print(foo.age)

    # 运行结果:
    {'name': 'sylar', 'age': 18}
    28

