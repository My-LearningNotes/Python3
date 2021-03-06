threading模块
=============

在Python中，通常使用\ ``threading``\ 模块来实现多线程编程。

在\ ``threading``\ 模块中，除了\ ``Thread``\ 类之外，还包括许多非常好的
同步机制。

+-----------------------+----------------------------------------------------------------------------------------+
| 对象                  | 描述                                                                                   |
+=======================+========================================================================================+
| ``Thread``            | 表示一个执行线程的对象                                                                 |
+-----------------------+----------------------------------------------------------------------------------------+
| ``Lock``              | 锁原语对象                                                                             |
+-----------------------+----------------------------------------------------------------------------------------+
| ``RLock``             | 可重入锁对象，使单一线程可以(再次)获得已持有的锁(递归锁)                               |
+-----------------------+----------------------------------------------------------------------------------------+
| ``Condition``         | 条件变量对象，使得一个线程等待另一个线程满足特定的“条件”，比如改变状态值或某个数据值   |
+-----------------------+----------------------------------------------------------------------------------------+
| ``Event``             | 条件变量的通用版本，任意数量的线程等待某个事件发生，在该事件发生后所有线程将被激活     |
+-----------------------+----------------------------------------------------------------------------------------+
| ``Semaphore``         | 信号量，为线程间共享的有限资源提供了一个“计数器”，如果没有可用资源时会被阻塞           |
+-----------------------+----------------------------------------------------------------------------------------+
| ``BoundedSemphore``   | 与Semaphore相似，不过它不允许超过初始值                                                |
+-----------------------+----------------------------------------------------------------------------------------+
| ``Timer``             | 与Thread类似，不过它要在运行前等待一段时间                                             |
+-----------------------+----------------------------------------------------------------------------------------+

``theading``\ 模块中的一些常用方法:

-  ``threading.current_thread()``

   Return the current ``Thread`` object.

-  ``threading.active_count()``

   Return the number of ``Thread`` objects currently alive.

-  ``threading.enumerate()``

   Return a list of all ``Thread`` objects currently alive.

``Thread``\ 类
--------------

``Thread``\ 类的常用属性和方法:

+--------------------------+------------------------------------------------------------------+
| 属性/方法                | 描述                                                             |
+==========================+==================================================================+
| ``name``                 | 线程名                                                           |
+--------------------------+------------------------------------------------------------------+
| ``ident``                | 线程的标识符                                                     |
+--------------------------+------------------------------------------------------------------+
| ``daemon``               | 布尔标志，表示这个线程是否是守护线程                             |
+--------------------------+------------------------------------------------------------------+
| ``__init__()``           | 构造函数                                                         |
+--------------------------+------------------------------------------------------------------+
| ``start()``              | 开始执行线程                                                     |
+--------------------------+------------------------------------------------------------------+
| ``run()``                | 定义线程功能的方法(通常在子类中被重写)                           |
+--------------------------+------------------------------------------------------------------+
| ``join(timeout=None)``   | d等待线程的终止，除非指定了\ ``timeout(秒)``\ ，否则会一直阻塞   |
+--------------------------+------------------------------------------------------------------+
| ``is_alive()``           | 布尔标志，表示这个线程是否还存活                                 |
+--------------------------+------------------------------------------------------------------+

-  获取或设置线程\ *name*\ ，直接操作\ *name*\ 属性

-  获取或设置守护线程，直接操作\ *daemon*\ 属性

使用\ ``Thread``\ 类创建线程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

使用\ ``Thread``\ 类，可以有很多方法来创建线程，两种比较常用的方法:

-  创建\ ``Thread``\ 类的实例，传给它一个函数及其参数，在该函数中定义线程要执行的操作

-  派生\ ``Thread``\ 类的子类并重写\ ``run()``\ 方法，实例化子类

实例化\ ``threading.Thread``\ 类并传入要运行的函数及其参数
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

实例化\ ``threading.Thread``\ 类，根据其构造函数:

-  通过关键字参数\ ``target``\ 定义要在线程中运行的函数

   .. code:: python

       target = function_to_run

-  通过关键字参数\ ``args``\ 定义一个列表或元组，表示传递给函数的位置参数

   .. code:: python

       args = [...]
       args = (...)

-  通过关键字参数\ ``kwargs``\ 定义传递给函数的关键字参数，关键字参数应该是一个\ *map*\ ，\ *key*\ 是字符串形式的函数的参数名

   .. code:: python

       kwargs = {'arg1': value1, 'arg2': value2, ...}

-  通过关键字参数\ ``name``\ 定义线程的名称

Example:

.. code:: python

    #! /usr/bin/env python3

    import threading
    import time

    def loop(arg1, arg2):
        print('thread {} is running ...').format(threading.current_thread().name)
        n = 0
        while n < 10:
            n += 1
            print('thread {} >>> {} {}'.format(threading.current_thread().name, arg1, arg2))
            time.sleep(1)
      	print('thread {} ended.'.format(threading.current_thread().name))
        
    print('thread {} is running ...'.format(threading.current_thread().name))

    t1 = threading.Thread(target=loop,
                          args=('hello', 'wrold'),
                          name='loop_thread_1')
    t2 = threading.Thread(target=loop,
                          kwargs={'args1': 'HELLO', 'args2': 'WORLD'},
                          name='loog_thread_2')
    t1.start()
    t2.start()
    print('count of running threads: {}'.format(threading.active_count()))
    print('list of running threads: {}'.format(threading.enumerate()))
    t1.join()
    t2.join()
    print('thread {} ended.'.format(threading.current_thread().name))

从\ ``threading.Thread``\ 类继承并重写\ ``run()``\ 方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: python

    #! /usr/bin/env python3

    import threading
    import time

    class MyThread(threading.Thread):
        
        def __init__(self):
            super(MyThread, self).__init__()
            
        def run(self):
            print('thread {} is running ...'.format(threading.current_thread().name))
            n = 0
            while n < 10:
                n += 1
                print('thread {} >>> {}'.format(threading.current_thread().name, n))
                time.sleep(1)
           	print('thread {} ended.'.format(threading.current_thread().name))
            
            
    print('thread {} is running ...'.format(threading.current_thread().name))
    t = MyThread()
    t.start()
    t.join()
    print('thread {} ended.'.format(threading.current_thread().name))
