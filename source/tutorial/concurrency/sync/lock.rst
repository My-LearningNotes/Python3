锁
==

锁有两种状态: **锁定**\ 和\ **未锁定**\ . 而且它也只支持两个函数: **获得锁**\ 和\ **释放锁**\ . 

当多线程争夺锁时, 允许第一个获得锁的线程进入临界区, 并执行代码. 
所有之后到达的线程都将被阻塞, 直到第一个线程执行结束, 退出临界区, 并释放锁. 
此时, 其它等待的线程可以获得锁并进入临界区. 
不过请记住, 那些被阻塞的线程是没有顺序的（即不是先到先执行), 胜出线程的选择是不确定的, 而且还会根据Python实现的不同而有所不同.


``threading.Lock``
------------------

原始锁是一个在锁定时不属于特定线程的同步基元组件. 

它有两个基本方法: ``acquire()``\ 和\ ``release()``\ . 
当状态为非锁定时, ``acquire()``\ 将状态改为\ *锁定*\ 状态并立即返回. 当状态是锁定时, ``acquire()``\ 将阻塞至其它线程调用\ ``release()``\ 将其改为非锁定状态, 然后\ ``acquire()``\ 调用重置其为锁定状态并返回. 
``release()``\ 只在锁定状态下调用, 它将状态改为非锁定并立即返回. 如果尝试释放一个非锁定的锁, 则会引发\ ``RuntimeError``\ 异常.

**所有方法的执行都是原子性的.**

一旦一个线程获得锁, 会阻塞随后尝试获得锁的线程, 直到它被释放; 任何线程都可以释放它(因为这个锁不属于特定线程).


* ``acquire(blocking=True, timeout=-1)``

可以阻塞或非阻塞地获得锁.

* ``release()``

释放一个锁. 这个锁可以在任何线程中调用, 不单指获得锁的线程. 

* ``locked()``

如果获得了锁则返回真值.


示例
----

.. code-block:: python
    :emphasize-lines: 10, 16, 18, 24, 26

    import threading


    class SharedCounter:
        """
        A counter object that can be shared by multiple threads.
        """
        def __init__(self, inital_value=0):
            self._value = inital_value
            self._value_lock = threading.Lock()  # 创建锁

        def incr(self,delta=1):
            """
            Increment the counter with locking
            """
            self._value_lock.acquire()  # 获得锁
            self._value += delta
            self._value_lock.release()  # 释放锁

        def decr(self, delta=1):
            """
            Decrement the counter with locking
            """
            self._value_lock.acquire()  # 获得锁
            self._value -= delta
            self._value_lock.release()  # 释放锁



使用上下文管理器
----------------

相比于显示调用锁的\ ``acquire()``\ 和\ ``release()``\ 方法, ``with``\ 语句更加优雅, 也更不容易出错, 
特别是程序员可能会忘记调用\ ``release()``\ 方法或者程序在获得锁之后产生异常这两种情况(使用\ ``with``\ 语句可保证在这两种情况下仍能正确释放锁).

使用\ ``with``\ 语句时, 此时每个对象的上下文管理器负责在进入该套件之前调用\ ``acquire()``\ 并在结束执行之后调用\ ``release()``\ .

``threading``\ 模块的对象\ ``Lock``\ , ``RLock``\ , ``Condition``\ , ``Semaphore``\ 和\ ``BoundedSemaphore``\ 都包含上下文管理器, 也就是说, 它们都可以使用\ ``with``\ 语句.

Example:

.. code-block:: python
    :emphasize-lines: 10, 16, 23

    import threading


    class SharedCounter:
        """
        A counter object that can be shared by multiple threads.
        """
        def __init__(self, inital_value=0):
            self._value = inital_value
            self._value_lock = threading.Lock()  # 获得锁

        def incr(self,delta=1):
            """
            Increment the counter with locking
            """
            with self._value_lock:
                self._value += delta

        def decr(self, delta=1):
            """
            Decrement the counter with locking
            """
            with self._value_lock:
                self._value -= delta

