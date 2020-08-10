条件变量
========

条件变量对应的对象是\ ``threading.Condition``\ .

* 条件变量是线程中的概念, 就是等待某一条件的发生, 和信号一样;
* 条件变量使线程睡眠(blocking), 等待某种条件的出现;
* 条件变量是利用线程间共享的\ **全局变量**\ 进行同步的一种机制, 主要包括两个动作:

    - 一个线程等待\ **条件变量**\ 的条件成立而挂起;
    - 另一个线程使\ *条件成立*\ (给出条件成立信号).

* 为了防止竞争, 条件变量的使用总是和一个\ **互斥锁**\ 结合在一起.


应用场景
--------

线程A需要等待某个条件成立才能继续往下执行, 条件不成立, 线程A阻塞等待, 当线程B执行过程中更改条件使之对线程A成立, 就唤醒线程A继续执行.

* 生产者/消费者模型.


对象的基本方法
--------------

* ``acquire()``\ : 获取锁.
* ``release()``\ : 释放锁.
* ``wait(timeout=None)``\ : 线程挂起直到被其它线程通知或超时. 如果调用此方法的线程没有获得锁, 将会引发\ ``RuntimeError``\ 异常. 
  调用此方法后, 这个线程就会释放锁, 然后挂起, 直到被其它线程调用\ ``notify()``\ 或\ ``notify_all()``\ 方法唤醒, 或者发生超时. 
  一旦被唤醒, 这个线程将重新获得锁并返回.
* ``wait_for(predicate, timeout=None)``\ : ``predicate``\ 是一个可调用对象, 返回的结果可别认为是一个布尔值. 线程调用此方法后, 将挂起直到条件为真.
* ``notify(n=1)``\ : 默认唤醒一个在此条件上等待的线程, 可以通过\ ``n``\ 来指定唤醒的线程数.
* ``notify_all()``\ : 唤醒所有的在此条件上等待的线程.

Example:

.. code-block:: python
    :emphasize-lines: 17,24, 27, 38, 47, 50, 55

    # 生产者消费者示例

    import threading
    import random
    import time


    class Producer(threading.Thread):
        def __init__(self, integers, condition):
            super().__init__()
            self.integers = integers
            self.condition = condition

        def run(self):
            while True:
                integer = random.randint(0, 256)
                self.condition.acquire()
                print(f'condition acquired by {self.name}')

                self.integers.append(integer)
                print(f'{integer} append to list by {self.name}')

                print(f'condition notified by {self.name}')
                self.condition.notify()

                print(f'condition released by {self.name}')
                self.condition.release()


    class Consumer(threading.Thread):
        def __init__(self,integers, condition):
            super().__init__()
            self.integers = integers
            self.condition = condition

        def run(self):
            while True:
                self.condition.acquire()
                print(f'condition acquired by {self.name}')

                while True:
                    if self.integers:
                        integer = self.integers.pop()
                        print(f'integer popped from list {self.name}')
                        break
                    print(f'condition wait for {self.name}')
                    self.condition.wait()

                    print(f'condition released by {self.name}')
                    self.condition.release()


    def main():
        integers = []
        condition = threading.Condition()
        t1 = Producer(integers, condition)
        t2 = Consumer(integers, condition)
        t1.start()
        t2.start()
        t1.join()
        t2.join()


    if __name__ == '__main__':
        main()

