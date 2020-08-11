``threading.Thread``
====================

``Thread``\ 类的常用属性和方法:

+--------------------------+------------------------------------------------------------------+
| 属性/方法                | 描述                                                             |
+==========================+==================================================================+
| ``name``                 | 线程名                                                           |
+--------------------------+------------------------------------------------------------------+
| ``ident``                | 线程的标识符                                                     |
+--------------------------+------------------------------------------------------------------+
| ``daemon``               | 布尔标志, 表示这个线程是否是守护线程                             |
+--------------------------+------------------------------------------------------------------+
| ``__init__()``           | 构造函数                                                         |
+--------------------------+------------------------------------------------------------------+
| ``start()``              | 开始执行线程                                                     |
+--------------------------+------------------------------------------------------------------+
| ``run()``                | 定义线程功能的方法(通常在子类中被重写)                           |
+--------------------------+------------------------------------------------------------------+
| ``join(timeout=None)``   | 等待线程的终止,除非指定了\ ``timeout(秒)``\ , 否则会一直阻塞     |
+--------------------------+------------------------------------------------------------------+
| ``is_alive()``           | 布尔标志, 表示这个线程是否还存活                                 |
+--------------------------+------------------------------------------------------------------+

- 获取或设置线程\ *name*\ ，直接操作\ *name*\ 属性;
- 获取或设置守护线程，直接操作\ *daemon*\ 属性.


使用\ ``Thread``\ 类创建线程
----------------------------

使用\ ``Thread``\ 类，可以有很多方法来创建线程:

    - 创建\ ``Thread``\ 类的实例，传给它一个函数及其参数，在该函数中定义线程要执行的操作;
    - 创建\ ``Thread``\ 类的实例, 传给它一个可调用的类实例; 
    - 派生\ ``Thread``\ 类的子类并重写\ ``run()``\ 方法，实例化子类.

常用的是第一个或第三个方法. 
当需要一个更加符合面向对象的接口时, 会选择前者; 否则会选择后者.


实例化\ ``threading.Thread``\ 类并传入要运行的函数及其参数
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

实例化\ ``threading.Thread``\ 类，根据其构造函数:

- 通过关键字参数\ ``target``\ 定义要在线程中运行的函数:

.. code-block:: python

    target = function_to_run

- 通过关键字参数\ ``args``\ 定义一个列表或元组，表示传递给函数的位置参数:

.. code-block:: python

    args = [...]
    args = (...)

- 通过关键字参数\ ``kwargs``\ 定义传递给函数的关键字参数，关键字参数应该是一个\ *map*\ ，\ *key*\ 是字符串形式的函数的参数名:

.. code-block:: python

    kwargs = {'arg1': value1, 'arg2': value2, ...}

- 通过关键字参数\ ``name``\ 定义线程的名称

Example:

.. code-block:: python
    :emphasize-lines: 17, 21, 24

    import threading
    from time import sleep, ctime

    loops = [4, 2]

    def loop(nloop, nsec):
        print(f'start loop {nloop} at: {ctime()}')
        sleep(nsec)
        print(f'loop {nloop} done at: {ctime()}')

    def main():
        print(f'starting at: {ctime()}')
        threads = []
        nloops = range(len(loops))

        for i in nloops:
            t = threading.Thread(target=loop, args=(i, loops[i]))
            threads.append(t)

        for i in nloops:  # start threads
            threads[i].start()

        for i in nloops:  # wait for all threads to finish
            threads[i].join()

        print(f'all DONE at: {ctime()}')

    if __name__ == '__main__':
        main()

.. note::

    通过\ ``target``\ 参数传入的函数, 其在新线程中执行.

实例化\ ``Thread``\ 对象后, 新线程并不会立即开始执行. 这是一个非常有用的同步功能, 尤其是当你并不希望线程立即开始执行时. 
当线程创建完成之后, 通过调用线程的\ ``start()``\ 方法让它们开始执行. 

在主线程中, 可以调用子线程的\ ``join()``\ 方法, 等待子线程结束, 或者在提供了超时时间的情况下, 达到超时退出. 
对于\ ``join()``\ 方法, 其另一个重要方面是其实它根本不需要调用. 
一旦线程启动, 它们就会一直执行, 直到给定的函数完成后退出. 
如果主线程还有其它事情要去做, 而不是等待这些线程完成, 就可以不调用\ ``join()``\ . 
``join()``\ 方法只有在需要等待线程完成的时候才是有用的.


创建\ ``threading.Thread``\ 类的实例, 传给它一个可调用的类实例
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在创建线程时, 与传入函数相似的一个方法是传入一个可调用的类的实例, 用于线程执行 --- 这种方法更加接近面向对象的多线程编程. 
这种可调用的类包含一个执行环境, 比起一个函数或者从一组函数中选择而言, 有更好的灵活性. 

.. note::

    所谓可调用的类实例, 是指实现了\ ``__call__()``\ 方法的类的实例.

Example:

.. code-block:: python
    :emphasize-lines: 28, 32, 35

    #!/usr/bin/env python3

    import threading
    from time import sleep, ctime

    loops = [4, 2]

    class ThreadFunc(object):
        def __init__(self, func, args, name=''):
            self.name = name
            self.func = func
            self.args = args

        def __call__(self):
            self.func(*self.args)

    def loop(nloop, nsec):
        print(f'start loop {nloop} at: {ctime()}')
        sleep(nsec)
        print(f'loop {nloop} done at: {ctime()}')

    def main():
        print(f'starting at: {ctime()}')
        threads = []
        nloops = range(len(loops))

        for i in nloops:  # create all threads
            t = threading.Thread(target=ThreadFunc(loop, (i, loops[i]), loop.__name__))
            threads.append(t)

        for i in nloops:  # start all threads
            threads[i].start()

        for i in nloops:  # wait for completion
            threads[i].join()

        print(f'all DONE at: {ctime()}')

    if __name__ == '__main__':
        main()

* 在上面的例子中, 在实例化\ ``Thread``\ 对象时, 同时实例化了可调用类\ ``ThreadFunc``\ , 并通过\ ``target``\ 参数将可调用的类实例传递给\ ``Thread``\ 对象;
* 我们希望这个类更加通用, 而不是局限于\ ``loop()``\ 函数, 因此添加了一些新的东西, 比如让这个类保存了函数的参数, 函数自身以及函数名的字符串, 而构造函数\ ``__init__()``\ 用于设定上述这些值;
* 当创建新线程时, ``Thread``\ 类的代码将调用\ ``ThreadFunc``\ 对象, 此时会调用\ ``__call()__``\ 这个特殊方法.

.. note::

    在可调用的类实例中, ``__call()__``\ 方法中的代码在新线程中执行, 而其它的代码仍然在主线程中执行.


从\ ``threading.Thread``\ 类继承并重写\ ``run()``\ 方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Example:

.. code-block:: python
    :emphasize-lines: 6, 16, 32, 36, 39

    import threading
    from time import sleep, ctime

    loops = [4, 2]

    class MyThread(threading.Thread):
        def __init__(self, func, args, name=''):
            threading.Thread.__init__(self)
            self.name = name
            self.func = func
            self.args = args

        def getResult(self):
            return self.res

        def run(self):
            print(f'starting {self.name} at: {ctime()}')
            self.res = self.func(*self.args)
            print(f'{self.name} finished at: {ctime()}')

    def loop(nloop, nsec):
        print(f'start loop {nloop} at: {ctime()}')
        sleep(nsec)
        print(f'loop {nloop} done at: {ctime()}')

    def main():
        print(f'starting at: {ctime()}')
        threads = []
        nloops = range(len(loops))

        for i in nloops:
            t = MyThread(loop, (i, loops[i]), loop.__name__)
            threads.append(t)

        for i in nloops:
            threads[i].start()

        for i in nloops:
            threads[i].join()

        for i in nloops:
            print(threads[i].getResult())  # 在主线程中执行

        print(f'all DONE at: {ctime()}')

    if __name__ == '__main__':
        main()

.. note::

    在继承\ ``Thread``\ 类的子类中, ``run()``\ 中的代码在新线程中执行, 其它部分的代码仍然在主线程中执行.
