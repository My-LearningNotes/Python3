``hasattr()``\ , ``getattr()``\ 和\ ``setattr()``\ 函数
=======================================================


``hasattr()``\ 函数
-------------------

``hasattr()``\ 函数用来判断某个类实例对象是否包含指定名称的属性或方法, 该函数的语法格式如下:

.. code-block:: python

    hasattr(obj, name)

其中, ``obj``\ 指的是某个类的实例对象, ``name``\ 表示指定的属性名或方法名, 根据判断的结果返回\ ``True``\ 或者\ ``False``\ .

无论方法名还是属性名, 都在\ ``hasattr()``\ 函数的匹配范围之内. 
但是, 我们只能通过该函数判断实例对象是否包含指定名称的属性或方法, 不能精确判断, 该名称代表的是属性还是方法.

Example:

.. code-block:: python
    :emphasize-lines: 10, 11, 12, 13

    class Foo:
        def __init__(self):
            self.name = 'sylar'
            self.age = 18

        def say(self):
            pass

    foo = Foo()
    print(hasattr(foo, 'name'))
    print(hasattr(foo, 'age'))
    print(hasattr(foo, 'say'))
    print(hasattr(foo, 'abc'))

    # 运行结果:
    True
    True
    True
    False


``getattr()``\ 函数
-------------------

``getattr()``\ 函数获取某个类实例对象中指定属性的值. 
和\ ``hasattr()``\ 函数不同, ``getattr()``\ 函数只会从类对象包含的属性中进行查找.

``getattr()``\ 函数的语法格式如下:

.. code-block:: python

    getattr(obj, name[,default])

其中, ``obj``\ 表示指定的类对象, ``name``\ 表示指定的属性名, 
而\ ``default``\ 是可选参数, 用于设定该函数的默认返回值, 即当函数查找失败时, 如果不指定\ ``defalut``\ 参数, 则程序将直接报\ ``AttributeError``\ 错误, 反之该函数将返回\ ``default``\ 指定的值.


``setattr()``\ 函数
-------------------

``setattr()``\ 函数的功能相对比较复杂, 它最基础的功能是修改对象中的属性值; 其次, 它还可以实现为对象动态添加属性或者方法.

``setattr()``\ 函数的语法格式如下:

.. code-block:: python

    setattr(obj, name, value)


* 修改对象的属性值;
* 使用\ ``setattr()``\ 函数对对象中的指定的属性或方法进行修改时, 如果该名称查找失败, Python解释器不会报错, 而是会给该对象动态添加指定的属性或方法.

