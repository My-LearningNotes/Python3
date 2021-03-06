Python类型转换
==============

Python提供了多种可以实现数据类型转换的函数:

.. table:: 常用数据类型转换函数

    ========================= =================================================
    函数                      作用
    ``int(x)``                将\ ``x``\ 转换成整数类型
    ``float(x)``              将\ ``x``\ 转换成浮点数类型
    ``complex(real[, imag])``  创建一个复数
    ``str(x)``                将\ ``x``\ 转换为字符串
    ``repr(x)``               将\ ``x``\ 转换为表达式字符串
    ``eval(str)``             计算在字符串中的有效Python表达式, 并返回一个对象
    ``chr(x)``                将整数\ ``x``\ 转换为一个字符
    ``ord(x)``                将一个字符\ ``x``\ 转换为它对应的整数值
    ``hex(x)``                将一个整数\ ``x``\ 转换为一个十六进制字符串
    ``oct(x)``                将一个整数\ ``x``\ 转换为一个八进制字符串
    ========================= =================================================

需要注意的是, 在使用类型转换函数时, 提供给它的数据必须时有意义的.

