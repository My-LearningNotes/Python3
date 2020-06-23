获取更多的异常信息
==================

在实际的调试过程中, 有时只获得异常的类型是远远不够的, 还需要借助更多的异常信息才能解决问题.

捕获异常时, 有两种方式可以获得更多的异常信息, 分别是:

    *   使用\ ``sys``\ 模块中的\ ``exc_info``\ 方法;
    *   使用\ ``traceback``\ 模块中的相关函数.


``sys.exc_info()``
------------------

模块\ ``sys``\ 中, 有两个方法可以返回异常的全部信息, 分别是\ ``ecx_info()``\ 和\ ``last_traceback()``\ , 这两个函数有相同的功能和用法.

``exc_info()``\ 方法会将当前的异常信息以元组的形式返回, 该元组中包含3个元素, 分别为\ ``type``\ , ``value``\ 和\ ``trackback``\ , 它们的含义分别是:

*   ``type``

异常类型的名称, 它是\ ``BaseException``\ 的子类.

*   ``value``

捕获到的异常实例.

*   ``trackback``

是一个\ ``trackback``\ 对象.

Example:

.. code-block:: python
    :emphasize-lines: 7

    import sys

    try:
        x = int(input('输入一个除数:'))
        print(100 / x)
    except:
        print(sys.exc_info())

当输入0时, 程序的运行结果为:

.. code-block:: text

    输入一个除数:0
    (<class 'ZeroDivisionError'>, ZeroDivisionError('division by zero',), <traceback object at 0x7f0fe28e3f08>)

要查看\ ``traceback``\ 对象包含的内容, 需要先导入\ ``traceback``\ 模块, 然后调用模块中的\ ``print_tb()``\ 方法, 并将\ ``sys.ecx_info()``\ 输出的\ ``traceback``\ 对象作为参数输入.

Example:

.. code-block:: python
    :emphasize-lines: 8

    import sys
    import traceback

    try:
        x = int(input('输入一个除数:'))
        print(100 / x)
    except:
        print(traceback.print_tb(sys.exc_info()[2]))

输出0, 程序运行结果为:

.. code-block:: text

    输入一个除数:0
    File "./info.py", line 8, in <module>
        print(100 / x)

可以看到, 输出信息中包含了更多的异常信息, 包括文件名, 抛出异常的代码所在的行数, 抛出异常的具体代码.


``trackeback``\ 模块
--------------------

使用\ ``traceback``\ 模块, 该模块可以用来查看异常的传播轨迹, 追踪异常触发的源头.

使用\ ``traceback``\ 模块查看异常传播轨迹, 首先需要导入\ ``traceback``\ 模块, 该模块提供了如下两个常用方法:

    *   ``traceback.print_exc()``\ : 将异常传播轨迹信息输出到控制台或指定文件中：
    *   ``traceback.format_exc()``\ : 将异常传播轨迹信息转换成字符串.

Example:

.. code-block:: python
    :emphasize-lines: 23, 26

    import traceback

    class SelfException(Exception):
        pass

    def main():
        f1()

    def f1():
        f2()

    def f2():
        f3()

    def f3():
        raise SelfException("自定义异常信息")

    if  __name__ == '__main__':
        try:
            main()
        except Exception:
            # 捕捉异常, 并将异常传播信息输出到控制台
            traceback.print_exc()

            # 捕捉异常, 并将异常传播信息输出到指定文件中
            traceback.print_exc(file=open('log.txt', 'a'))

    运行结果为:
    Traceback (most recent call last):
    File "./trace.py", line 22, in <module>
        main()
    File "./trace.py", line 9, in main
        f1()
    File "./trace.py", line 12, in f1
        f2()
    File "./trace.py", line 15, in f2
        f3()
    File "./trace.py", line 18, in f3
        raise SelfException("自定义异常信息")
    SelfException: 自定义异常信息
