事件对象
========

这是线程间通信的最简单的机制之一: **一个线程发出事件信号, 而其它线程等待该信号.**

一个事件对象管理一个内部标志, 调用\ ``set()``\ 方法可将其设置为\ ``true``\ , 调用\ ``clear()``\ 方法可将其设置为\ ``false``\ , 调用\ ``wait()``\ 方法将进入阻塞直到标志为\ ``true``\ .

.. code-block:: python

    threading.Event

实现事件对象的类.

事件对象管理一个内部标志, 调用\ ``set()``\ 方法可将其设置为\ ``true``\ , 调用\ ``clear()``\ 方法可将其设置为\ ``false``\ , 调用\ ``wait()``\ 方法将进入阻塞直到标志为\ ``true``\ . 这个标志初始时为\ ``false``\ .

* ``is_set()``

当且仅当内部标志为真时返回\ ``True``\ .

* ``set()``

将内部标志设置为\ ``true``\ . 
所有正在等待这个事件的线程将被唤醒. 
当表示为\ ``true``\ 时, 调用\ ``wait()``\ 方法的线程不会阻塞.

* ``clear()``

将内部标志设置为\ ``false``\ . 
之后调用\ ``wait()``\ 方法的线程将会被阻塞, 直到调用\ ``set()``\ 方法将内部标志再次设置为\ ``true``\ .

* ``wait(timeout=None)``

阻塞线程直到内部变量为\ ``true``\ .

如果调用调用时内部标志为\ ``true``\ , 将立即返回. 否则将阻塞线程, 直到调用\ ``set()``\ 方法将标志设置为\ ``true``\ 或者超时.

超时返回\ ``False``\ , 其它情况下总是返回\ ``True``\ .


Example:

.. code-block:: python
    :emphasize-lines: 19, 37, 39

    #!/usr/bin/env python3

    import time
    from threading import Thread, Event
    import random

    items = []
    event = Event()

    class Consumer(Thread):
        def __init__(self, items, event):
            super().__init__()
            self.items = items
            self.event = event

        def run(self):
            while True:
                time.sleep(2)
                self.event.wait()
                item = self.items.pop()
                print(f'Consumer notify: {item} popped from list by {self.name}')

    class Producer(Thread):
        def __init__(self, items, event):
            super().__init__()
            self.items = items
            self.event = event

        def run(self):
            global item
            for i in range(100):
                time.sleep(2)
                item = random.randint(0, 256)
                self.items.append(item)
                print(f'Producer notify: {item} appended to list by {self.name}')
                print(f'Producer notify: event set by {self.name}')
                self.event.set()
                print(f'Producer notify: event cleared by {self.name}')
                self.event.clear()


    if __name__ == '__main__':
        t1 = Producer(items, event)
        t2 = Consumer(items, event)
        t1.start()
        t2.start()
        t1.join()
        t2.join()

