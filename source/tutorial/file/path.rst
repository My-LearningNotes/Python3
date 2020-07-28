文件路径
========

关于文件, 它有两个关键属性: "文件名"和"路径". 
其中, 文件名指的是为每个文件设定的名称, 而路径则用来指明文件在计算机上的位置. 


Windows上的反斜杠以及Linux上的正斜杠
------------------------------------

在Windows上, 路径书写使用反斜杠\ ``\``\ 作为文件夹之间的分隔府. 
而在Linux上, 使用正斜杠\ ``/``\  作为它们的路径分隔符. 
如果想要程序能同时在Windwos和Linuxs系统上运行, 在编写Python脚本时, 就必须处理这两种情况.

好在, 用\ ``os.path.join()``\ 函数来做这件事很简单. 
如果将单个文件和路径上的文件夹名称的的字符串传递给它, ``os.path.join()``\ 会返回一个文件路径的字符串, 包含正确的路径分隔符.

Example:

.. code-block:: python
    :emphasize-lines: 2

    >>> import os
    >>> os.path.join('a', 'b', 'c')
    'a/b/c'

上述代码是在Linux系统上运行的, 所以输出的是\ ``a/b/c``\ , 如果是在Windows系统上运行则输出为\ ``a\b\c``\ .


当前工作目录
------------

当前工作目录(Current Working Directory, CWD). 

在Python中, 可以使用\ ``os.getcwd()``\ 函数获取当前的工作路径, 还可以使用\ ``os.chdir()``\ 来改变它. 

Example:

.. code-block:: python
    :emphasize-lines: 2, 4

    >>> import os
    >>> os.getcwd()
    '/home/sylar/Notes/Python3'
    >>> os.chdir('/tmp')
    >>> os.getcwd()
    '/tmp'
    >>>


相对路径和绝对路径
------------------

**绝对路径**\ : 从根目录开始的路径;
**相对路径**\ : 相对于当前工作目录的位置.

``os.path``\ 模块提供了一些函数, 可以实现绝对路径和相对路径之间的转换, 以及检查给定的路径是否为绝对路径.

    * ``os.path.abspath(path)``

    返回\ ``path``\ 参数的绝对路径.

    * ``os.path.isabs(path)``

    判断参数\ ``path``\ 是否是绝对路径.

    * ``os.path.relpath(path, start)``

    返回从\ ``start``\ 到路径\ ``path``\ 的相对路径, 如果没有提供\ ``start``\ , 就使用当前工作目录作为开始路径.

    * ``os.path.dirname(path)`` 

    返回路径, 即\ ``path``\ 参数中最后一个斜杠之前的所有内容.

    * ``os.path.basename(path)``\

    返回文件名, 即\ ``path``\ 参数中最后一个斜杠之后的所有内容.

    * ``os.path.split(path)``

    返回一个包含两个字符串的元组, 第一个是\ ``path``\ 的\ ``dirname``\ , 第二个是\ ``basename``\ .

如果提供的路径不存在, 许多Python函数就会报错并崩溃, 但好在\ ``os.path``\ 模块提供了以下函数用于检测给定的路径是否存在, 以及它是文件还是文件夹:

    * ``os.path.exists(path)``

    判断指定的文件或文件夹是否存在.

    * ``os.path.isfile(path)``

    判断是否存在且是文件.

    * ``os.path.isdir(path)``

    判断是否存在且是文件夹.

