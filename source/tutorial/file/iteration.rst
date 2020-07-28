迭代文件内容
============

一种常见的文件操作是迭代其内容, 并在迭代过程中反复采取某种措施.


每次一个字符(或字节)
---------------------

一种最简单(也可能是最不常见)的文件内容迭代方式是: 在\ ``while``\ 循环中使用方法\ ``read()``\ .

Example:

.. code-block:: python

    with open(filename) as f:
        char = f.read(1)
        while char:
            process(char)
            char = f.read(1)

上面的代码之所以可行, 是因为到达文件结尾时, 方法\ ``read()``\ 将返回一个空字符串. 
但在此之前, 返回的字符串都只包含一个字符(对应于布尔值\ ``True``).

但是上面的代码中\ ``char = f.read(1)``\ 语句出现了两次, **代码重复不是一件好事情**. 
可以改进为使用\ ``while True/break``\ 技巧:

.. code-block:: python

    with open(filename) as f:
        while True:
            char = f.read(1)
            if not char:
                break
            process(char)


每次一行
--------

处理文本文件时, 通常想做的是迭代其中的行, 而不是每个字符, 可以在\ ``while``\ 循环中使用\ ``readline()``\ 方法:

.. code-block:: python

    with open(filename) as f:
        while True:
            line = f.readline()
            if not line:
                break
            process(line)


读取所有文件内容
----------------

如果文件不太大, 可一次读取整个文件. 
为此, 可使用方法\ ``read()``\ 并不提供任何参数(将整个文件读取到一个字符串中), 也可使用方法\ ``readlines()``\ (将文件读取到一个字符串列表中, 其中每个元素都是一行).
通过这样的方式读取文件, 可轻松地迭代字符和行.

Example:

.. code-block:: python

    # 使用read迭代字符
    with open(filename) as f:
        for char in f.read():
            process(char)

    # 使用readlines迭代行
    with open(filename) as f:
        for line in f.readlines():
            process(line)


使用\ ``fileinput``\ 实现延迟行迭代
-----------------------------------

有时候需要迭代大型文件中的行, 此时使用\ ``readlines()``\ 将占用太多内存. 
当然, 可以转而结合使用\ ``while``\ 循环和\ ``readline()``\ . 
但在Python中, 在可能的情况下, 应首选\ ``for``\ 循环.

可以使用一种称为\ **延迟行迭代**\ 的方法, 说它延迟是因为它只读取实际需要的文本部分.

Example:

.. code-block:: python

    import fileinput
    
    # 使用fileinput实现延迟行迭代
    for line in fileinput.input(filename):
        process(line)

    
文件迭代器
----------

**文件实际上也是可迭代的**, 这意味着可在\ ``for``\ 循环中直接使用它们来迭代行, **这也是最常用的方法**.

Example:

.. code-block:: python

    with open(filename) as f:
        for line in f:
            process(line)

* 与其它文件一样, ``sys.stdin``\ 也是可迭代的, 因此要迭代标准输入中的所有行, 可像下面这么做:

.. code-block:: python

    import sys

    for line in sys.stdin:
        process(line)

* 因为文件对象也是可迭代的, 可对迭代器做的事情基本上都可以对文件做, 如(使用\ ``list(open(filename))``\ )将其转换为字符串列表, 其效果与使用\ ``readlines()``\ 相同.

