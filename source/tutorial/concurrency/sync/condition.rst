条件变量
========

* 条件变量是线程中的概念, 就是等待某一条件的发生, 和信号一样;
* 条件变量使线程睡眠(blocking), 等待某种条件的出现;
* 条件变量是利用线程间共享的\ **全局变量**\ 进行同步的一种机制, 主要包括两个动作:

    - 一个线程等待\ **条件变量**\ 的条件成立而挂起;
    - 另一个线程使\ *条件成立*\ (给出条件成立信号).

* 为了防止竞争, 条件变量的使用总是和一个\ **互斥锁**\ 结合在一起.


条件变量总是与某种类型的锁对象相关联, 锁对象可以通过传入获得, 或者在缺省的情况下自动创建. 
当多个条件变量需要共享同一个锁时, 传入一个锁很有用. 
锁是条件变量的一部分, 你不必单独地跟踪它.


``threading.Condition(lock=None)``
----------------------------------

实现条件变量对象的类. 

一个条件变量对象允许一个或多个线程在被其它线程所通知之前挂起等待, 通知之后唤醒.

如果给出了非\ ``None``\ 的\ ``lock``\ 参数, 则它必须为\ ``Lock``\ 或\ ``RLock``\ 对象, 并且它将被用作底层锁. 
否则, 将会创建新的\ ``RLock``\ 对象, 并将其用作底层锁.

* ``acquire(*args)`` 
  
请求底层锁.

* ``release()`` 

释放底层锁.

* ``wait(timeout=None)``
  
线程挂起直到被其它线程通知或超时. 如果调用此方法的线程没有获得锁, 将会引发\ ``RuntimeError``\ 异常. 
调用此方法后, 这个线程就会释放锁, 然后挂起, 直到被其它线程调用\ ``notify()``\ 或\ ``notify_all()``\ 方法唤醒, 或者发生超时. 
一旦被唤醒, 这个线程将重新获得锁并返回.

可以使用\ ``timeout``\ 参数设置超时. 如果设置了\ ``timeout``\ , 则超时返回\ ``False``\ , 否则返回\ ``True``\ .

* ``wait_for(predicate, timeout=None)``

等待, 直到条件计算为真. 

``predicate``\ 应该是一个可调用对象, 且它的返回值可被解释为一个布尔值.

这个方法会重复调用\ ``wait()``\  直到满足判断式或者发生超时. 

忽略超时功能, 调用此方法大致相当于:

.. code-block:: python

    while not predicate():
        cv.wait()

* ``notify(n=1)``\ : 默认唤醒一个在此条件上等待的线程, 可以通过\ ``n``\ 来指定唤醒的线程数.
* ``notify_all()``\ : 唤醒所有的在此条件上等待的线程.

.. note::

    ``notify()``\ 和\ ``notify_all()``\ 方法并不会释放锁, 这意味着被唤醒的线程不会立即从它们的\ ``wait()``\ 方法调用中返回, 
    而是会在调用了\ ``notify()``\ 方法或\ ``notify_all()``\ 方法的线程最终放弃了锁的所有权后返回.

选择\ ``notify()``\ 还是\ ``notify_all()``\ , 取决于一次状态改变是只能被一个还是能被多个等待线程锁用. 
例如, 在一个典型的生产者 - 消费者情形中, 添加一个项目到缓冲区只需唤醒一个消费者线程.


应用场景
--------

使用条件变量的典型风格是用于同步某些共享状态, 那些对状态的某些特定改变感兴趣的线程, 它们重复调用\ ``wait()``\ 方法, 直到看到所期望的改变发生; 
而对于修改线程状态的线程, 它们将当前状态改变为可能是等待者所期待的状态后, 调用\ ``notify()``\ 或者\ ``notify_all()``\ 方法.

例如, 下面的代码是一个通用的无限缓冲区容量的生产者 - 消费者模型:

.. code-block:: python

    # Consume one item
    with cv:
        while not an_item_is_available():
            cv.wait()
        get_an_available_item()


    # Produce one item
    with cv:
        make_an_item_available()
        cv.notify()

**使用**\ ``while``\ **循环检查所要求的条件成立与否是有必要的**\ , 因为\ ``wait()``\ 方法可能经过不确定长度的时间后才会返回, 
而此时导致\ ``notify()``\ 方法调用的那个条件可能已经不再成立. 这是多线程编程所固有的问题.
``wait_for()``\ 方法可自动化条件的检查, 并简化超时计算.

.. code-block:: python

    # Consume an item
    with cv:
        wait_for(an_item_is_availale)
        get_an_available_item()


上下文管理器
------------

``threading.Condition``\ 支持上下文管理器, 可以使用\ ``with``\ 语法.

.. code-block:: python

    condition = threading.Condtion()
    with condition:
    # do something...

大致相当于:

.. code-block:: python

    condition = threading.Condition()
    condition.acquire()
    # do something...
    condition.release()


示例
----

生产者-消费者的例子:

.. code-block:: python

    #!/usr/bin/env python3

    import threading
    import time


    items = []
    condition = threading.Condition()


    class Producer(threading.Thread):
        def __init__(self):
            super().__init__()

        def produce(self):
            global condition
            global items
            with condition:
                if len(items) == 10:
                    condition.wait()
                    print('Producer notify: items produced are ' + str(len(items)))
                    print('Producer notify: stop the production!!!')
                items.append(1)
                print('Producer notify: total items produced ' + str(len(items)))
                condition.notify()

        def run(self):
            for i in range(20):
                time.sleep(1)
                self.produce()
        

    class Consumer(threading.Thread):
        def __init__(self):
            super().__init__()

        def consume(self):
            global condition
            global items
            with condition:
                if len(items) == 0:
                    condition.wait()
                    print('Consumer nofity: no item to consume')
                items.pop()
                print('Consumer notify: consumed 1 item')
                print('Comsumer notify: items to consume are ' + str(len(items)))

                condition.notify()

        def run(self):
            for i in range(20):
                time.sleep(2)
                self.consume()

    def main():
        producer = Producer()
        consumer = Consumer()
        producer.start()
        consumer.start()
        producer.join()
        consumer.join()

    if __name__ == '__main__':
        main()

