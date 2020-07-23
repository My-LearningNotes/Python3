迭代器
======


迭代器协议
----------

**迭代(iterate)**\ 意味着重复多次, 就像循环那样.
我们可以使用\ ``for``\ 循环迭代序列和字典, 但实际上也可迭代其它对象: **实现了**\ ``__iter__``\ **方法的对象**\ .

.. note::
    
    可迭代对象和迭代器之间还是有区别的, 虽然它们可以是同一个对象.

    实现\ ``__iter__``\ 方法的对象, 称为\ **可迭代对象**\ , ``__iter__``\ 方法返回一个迭代器. 
    实现了\ ``__next__``\ 方法的对象, 称为\ **迭代器**\ . 
    如果\ ``__iter__``\ 方法中返回的是\ ``self``\ , 那么可迭代对象和迭代器就是同一个对象.

方法\ ``__iter__``\ 返回一个迭代器, 迭代器是包含方法\ ``__next__``\ 的对象, 而调用这个方法时可不提供任何参数. 
当调用方法\ ``__next__``\ 时, 迭代器返回其下一个值. 
如果迭代器没有可供返回的值, 应引发\ ``StopIteration``\ 异常(预示迭代的结尾).

* 对于可迭代对象, 还可以使用内置函数\ ``iter``\ 获取它的迭代器. 对于可迭代对象\ ``obj``\ , ``iter(obj)``\ 与\ ``obj.__iter__()``\ 等效;

* 对于迭代器, 还可以调用内置函数\ ``next``\ . 对于迭代器\ ``it``\ , ``next(it)``\ 与\ ``it.__next__()``\ 等效.

对于一个可迭代对象, 我们可以使用\ ``for``\ 循环对其迭代; 
在知道了迭代的原理之后, 我们还可以手动对其进行迭代.

例如, 使用\ ``for``\ 循环读取一个文件中所有的行:

.. code-block:: python

    # 使用for循环进行迭代
    def for_iter():
        with open('/etc/passwd') as f:
            for line in f:
                print(line, end='')

手动读取一个文件中所有的行:

.. code-block:: python

    def manual_iter():
        with open('/etc/passwd') as f:
            try:
                while True:
                    line = next(f)
                    print(line, end='')
            except StopIteration:
                pass

通常, 迭代时使用\ ``StopIteration``\ 异常来指示异常的结尾. 
然而, 使用内置函数\ ``next()``\ 时, 还可以通过返回一个指定值来标记结尾, 比如\ ``None``\ :

.. code-block:: python  

    def manual_iter():
    with open('/etc/passwd') as f:
        while True:
            line = next(f, None)
            if line is None:
                break
            print(line, end='')

.. note::

    使用迭代器, 每次获取一个值, 可以逐个的对值进行处理.
    使用列表等序列, 是先把所有的值都获取, 然后再逐个处理. 
    相比较而言, 使用迭代器更通用, 更简单, 更优雅.

Example:

.. code-block:: python

    # 用迭代器实现的斐波那契数列
    class Fibs:
        def __init__(self):
            self.a = 0
            self.b = 1

        def __next__(self):
            self.a, self.b = self.b, self.a + self.b
            return self.a

        def __iter__(self):
            return self

上面的例子是用迭代器实现的斐波那契数列, 如果用列表实现的话, 这个列表的长度必须是无穷大的.

注意, 上面例子中的迭代器实现了方法\ ``__iter__``\ , 而这个方法返回迭代器本身. 
在很多情况下, 都在另一个对象中实现返回迭代器的方法\ ``__iter__``\ . 
但推荐在迭代器中也实现\ ``__iter__``\ (返回它自己), 这样迭代器就可直接用于for循环中了.

可迭代对象和迭代器可以是分开的两个对象, 也可以是同一个对象.
如果它们是分开的两个对象, 推荐在迭代器中也实现\ ``__iter__``\ 方法, 并返回迭代器自己.
这样, 不仅可迭代对象可以用for循环迭代, 迭代器也可以.

例如, 对于上面用迭代器实现的斐波那契数列, 将可迭代对象和迭代器分开实现:

.. code-block:: python

    # 可迭代对象
    class Fibs:
        def __init__(self):
            self.a = 0
            self.b = 1

        def __iter__(self):
            return FibsIterator(self)

    # 迭代器
    class FibsIterator:
        def __init__(self, obj):
            self._obj = obj
            self._a = obj.a
            self._b = obj.b

        def __next__(self):
            self._a, self._b = self._b, self._a + self._b
            return self._a



从迭代器创建序列
----------------

除了对迭代器和可迭代对象进行迭代之外, 还可以将它们转换为序列. 
在可以使用序列的情况下, 大多也可以使用迭代器或可迭代对象(诸如索引和切片等操作除外). 

可以使用\ **序列的构造函数**\ , 如\ ``list()``\, ``tuple()``\ 等将迭代器转换为序列.

Example:

.. code-block:: python

    class TestIterator:
        value = 0
        def __next__(self):
            self.value += 1
            if self.value > 10:
                raise StopIteration
            return self.value

        def __iter__(self):
            return self

    ti = TestIterator()
    print(list(ti))

    # 运行结果:
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

