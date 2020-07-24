Python的\ ``with``\ 语法
========================

资源的管理在程序设计中是一个很常见的问题, 例如: 文件的打开和关闭, 网络socket, 各种锁等, 最主要的问题点在于我们要确保这些开启的资源在使用完之后被关闭(释放). 
如果忘记关闭这些资源, 就会造成程序执行的性能问题, 甚至出现错误, 而除了关闭之外, 有些特殊的资源在使用完之后, 还必须进行一些清理操作, 这些也是资源管理上需要注意的.

Python语言提供了\ ``with``\ 这个独特的语法, 可让程序设计者更容易管理开启的资源, 在这样的语法架构下, Python会自动进行资源的建立, 清理与回收操作, 让程序设计者在使用各种资源时更为方便.


``with``\ 基本语法
------------------

传统上, 要打开一个文件, 我们会这样写:

.. code-block:: python

    # 打开文件
    f = open(filename)
    ...

    # 关闭文件
    f.close()

这种写法有一个问题, 如果在使用文件的过程中发生了一些意外状况, 造成程序的异常退出, 这个打开的文件就会没有关闭, 所以比较好的程序写法是使用\ ``try``\ 与\ ``finally``\ .

.. code-block:: python

    f = open(filename)
    try:
        ...
    finally:
        f.close()

上面的这种写法虽然不会有问题, 但是缺点就是必须手动加入关闭文件的代码, 不是很方便, 也很容易忘记.
这种情况我们可以改用\ ``with``\ 的写法:

.. code-block:: python

    # 以with打开文件
    with open(filename) as f:
        ...

这里使用\ ``with``\ 打开文件, 并将打开的文件关联到变量\ ``f``\ , 但是这个\ ``f``\ 只有在\ ``with``\ 的范围之内可以使用, 而离开这个范围时\ ``f``\ 就会自动关闭, 回收资源.

.. note::

    可以这么理解, ``with``\ 会创建一个局部的作用域, 当离开这个局部作用域时, 其中的资源会自动释放.


``with``\ 的常用情景
--------------------

* 打开文件
* 网络资源
* 线程锁


自定义\ ``Context Manager``
---------------------------

自定义类实现\ ``Context Manager``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``Context Manager``\ 是一个实现了\ ``__enter__()``\ 和\ ``__exit__()``\ 方法的类, 在\ ``__enter__()``\ 中创建并返回资源, 在\ ``__exit__()``\ 中执行清理操作并回收资源. 

可以结合使用\ ``with``\和\ ``Context Manager``\ 来实现对资源的自动管理. 
``with``\ 在刚开始执行时, 会执行\ ``__enter__()``\ 方法, 返回分配的资源(如打开文件), 而在离开\ ``with``\ 的作用域时, 会自动调用\ ``__exit__()``\ 方法释放资源(如关闭文件).

Example:

.. code-block:: python
    :emphasize-lines: 6, 11

    class File:
        def __init__(self, filename, mode):
            self._filename = filename
            self._mode = mode

        def __enter__(self):
            print('open file')
            self.open_file = open(self._filename, self._mode)
            return self.open_file

        def __exit__(self, type, value, traceback):
            print('close file')
            self.open_file.close()


    if __name__ == '__main__':
        with File('/tmp/test.txt', 'w') as f:
            print('write file')
            f.write('hello, world')


使用\ ``contextlib``\ 模块生成\ ``Context Manager``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 定义一个生成器函数, 在生成器函数中使用\ ``yield``\ 语法返回初始时创建的资源, 在\ ``yield``\ 语句之后释放资源;
* 使用\ ``contextlib``\ 模块中的装饰器\ ``@contextmanager``\ 装饰上面定义的生成器函数, 这样就生成了一个\ ``Context Manager``\ .

Example:

.. code-block:: python
    :emphasize-lines: 3, 5, 6, 7

    from contextlib import contextmanager

    @contextmanager
    def open_file(filename, mode):
        f = open(filename, mode) # 分配资源
        yield f                  # 返回资源
        f.close()                # 释放资源


    if __name__ == '__main__':
        with open_file('/tmp/test.txt', 'w') as f:
            f.write('HELLO, WORLD!')

