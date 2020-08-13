``threading.Timer``
===================

``Timer``\ 类是\ ``Thread``\ 类的子类, 因此可以像一个自定义线程一样工作. 

与线程一样, 通过调用\ ``start()``\ 方法启动计时器. 
而\ ``cancel()``\ 方法可以停止计时器(在计时结束之前), 计时器在执行其操作之前等待的时间间隔可能与用户指定的时间间隔不完全相同. 

.. code-block:: python

    threading.Timer(interval, function, *args=None, **kwargs=None)

创建一个计时器对象, 在经过\ ``interval``\ 秒的间隔时间后, 将会用参数\ ``args``\ 和关键字\ ``kwargs``\ 调用\ ``function``\ (注意, 只执行一次就结束, 而不是周期性执行).

.. note::

    计时器, 在等待一段时间间隔后, **执行一次**;

    定时器, 每隔一段时间, **周期性**\ 执行.


* ``cancel()``

停止计时器并取消执行计时器要执行的操作. 
仅当计时器仍处于等待状态时有效. 

Examle:

.. code-block:: python

    def hello():
        print('hello, world')

    t = Timer(10, hello)
    t.start()

