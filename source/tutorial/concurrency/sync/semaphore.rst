信号量
======

信号量(Semaphore)是最古老的同步原语之一.

**信号量就是一个计数器(用于标明当前的共享资源可以有多少并发访问), 当资源消耗时递减, 当资源释放时递增.**
消耗资源使计数器递减的操作习惯上称为\ ``P()``\ , 释放资源使计数器递增的操作一般称为\ ``V()``\ . 

Python简化了所有的命名, 使用和锁的函数/方法一样的名字: ``acquire``\ 和\ ``release``\ . 
信号量比锁更加灵活, 因为可以有多个线程, 每个线程拥有有限资源的一个实例. 

.. note::

    锁, 在任意时刻只允许有一个线程访问共享资源. 

    信号量, 可以允许多个线程同时访问共享资源. 
    信号量的一个特殊用途是互斥量. 互斥量是初始值为1的信号量, 可以实现数据, 资源的互斥访问.

信号量的使用过程一般如下:

    * 每当线程想要访问关联信号量的共享资源时, 调用\ ``acquire()``\ 方法申请资源;
    * 当不在需要使用共享资源时, 调用\ ``release()``\ 方法释放资源, 这会让信号量内部的计数器递增.

.. note::

    如果\ ``P()``\ 和\ ``V()``\ 都是原子操作, 如上所述的信号量的使用是没有问题的; 
    但如果不是, 则会产生问题.


``threading.Semaphore(value=1)``
--------------------------------

该类实现信号量对象, 管理一个原子性的时钟.

可选参数\ ``value``\ 赋予内部计数器初始值, 默认为1. 
如故\ ``value``\ 被赋予小于0的值, 将会引发\ ``ValueError``\ 异常.

.. note::

    ``acquire()``\ 方法和\ ``release()``\ 方法都是原子操作.

* ``acquire(blocking=True, timeout=None)``

获取一个信号量.

在不带参数的情况下调用时:

    - 如果在进入时内部计数器的值大于零, 则将其减一并立即返回True;
    - 如果在进入时内部计数器的值为零, 则将会阻塞直到被对\ ``release()``\ 的调用唤醒. 
      一旦被唤醒(并且计数器的值大于0), 则将计数器减1并返回True. 
      每次对\ ``release()``\ 的调用将只唤醒一个线程. 
      线程被唤醒的次序是不确定的.

当调用时将\ ``blocking``\ 参数设置\ ``False``\ , 则不进行阻塞.

调用时如果\ ``timeout``\ 参数不为\ ``None``\ , 则它将阻塞最多\ *timeout*\ 秒. 
请求在此时段内未能成功完成获取将返回\ ``False``\ , 在其它情况下返回\ ``True``\ .

* ``release()``

释放一个信号量, 将内部计数器的值加1. 
当计数器原先的值为0且有其它线程正在等待它再次大于0时, 唤醒正在等待的线程.


``threading.BoundedSemaphore(value=1)``
---------------------------------------

该类实现有界信号量. 
有界信号量通过检查以确保它当前的值不会超过初始值. 如果超过了初始值, 将会引发\ ``ValueError`` 异常. 
在大多数情况下, 信号量用于保护数量有限的资源. 
如果信号量被释放的次数过多, 则表明出现了错误. 
没有指定时, ``value``\ 的值默认为1.


示例
----

模拟一个简化的糖果机, 这个特制的机器只有5个可用的槽来保持库存(糖果). 
如果所有的槽都满了, 糖果就不能再加到这个机器中了; 
相似地, 如果每个槽都空了, 想要购买的消费者就无法买到糖果了. 
我们可以使用信号量来跟踪这些有限的资源(糖果槽).

.. code-block:: python
    :emphasize-lines: 18, 28

    #!/usr/bin/env python3

    from random import randrange
    from threading import BoundedSemaphore, Lock, Thread
    from time import sleep, ctime


    lock = Lock()
    MAX = 5
    candytray = BoundedSemaphore(MAX)

    def refill():
        # 这里的锁, 和信号量没有关系, 是为了使refill和下面的buy操作串行化
        # 即refill和buy不能同时进行
        lock.acquire()
        print('Refilling cady ...')
        try:
            candytray.release()
        except ValueError:
            print('full, skipping')
        else:
            print('OK')
        lock.release()

    def buy():
        lock.acquire()
        print('Buying candy ...')
        if candytray.acquire(blocking=False):
            print('OK')
        else:
            print('empty, skipping')
        lock.release()

    def producer(loops):
        for i in range(loops):
            refill()
            sleep(randrange(3))

    def consumer(loops):
        for i in range(loops):
            buy()
            sleep(randrange(3))

    def main():
        print(f'starting at: {ctime()}')
        nloops = randrange(2, 6)
        print(f'THE CANDY MACHINE (full with {MAX} bars)!')
        t1 = Thread(target=consumer, args=(randrange(nloops, nloops+MAX+2),))
        t2 = Thread(target=producer, args=(nloops,))
        t1.start()
        t2.start()


上下文管理器
------------

信号量对象也支持上下文管理器.

.. code-block:: python

    with semaphore:
        # do something...

相当于:

.. code-block:: python

    semaphore.acqurie()
    try:
        # do something...
    finally:
        semaphore.release()

只有在同一个线程中申请/释放资源时, 才适合使用上下文管理器. 
在\ *生产者-消费者*\ 这种模型中, 在一个线程中申请资源, 在另一个线程中释放资源, 就不适合使用上下文管理器.

