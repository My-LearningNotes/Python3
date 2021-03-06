Python异常处理机制
==================

程序运行时经常会碰到一些错误, 例如除数为0, 年龄为负数, 数组下标越界等, 这些错误如果不能发现并加以处理, 很可能会导致程序崩溃.
和C++, Java等编程语言一样, Python也提供了异常处理机制.

借助异常处理机制, 甚至在程序崩溃前也可以做一些必要的工作, 例如将内存中的数据写入文件, 关闭打开的文件, 释放分配的内存等.

Python异常处理机制会涉及\ ``try``\ , ``except``\ , ``else``\ , ``finally``\ 这4个关键字, 同时还提供了可主动使程序引发异常的\ ``raise``\ 语句.


.. toctree:: 
    :maxdepth: 1

    intro
    except_handling
    customize
    more_info
    guideline

