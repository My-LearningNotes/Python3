Python ``eval()``\ 和\ ``exec()``\ 函数
=======================================

``eval()``\ 和\ ``exec()``\ 函数都属于Python的内置函数, 它们的功能是类似的, 都可以执行一个字符串形式的Python代码(代码以字符串的形式提供), 相当于一个Python的解释器.
二者的不同之处在于, ``eval()``\ 执行完要返回结果, 而\ ``exec()``\ 执行完不返回结果.


用法
----

``eval()``\ 函数的语法格式为:

.. code-block:: python

    eval(expression, global=None, local=None, /)

而\ ``exec()``\ 函数的语法格式如下:

.. code-block:: python

    exec(expression, global=None, local=None, /)

可以看到, 二者的语法格式除了函数名, 其它都相同, 其中各个参数的具体含义如下:

*   ``expression``

这个参数是一个字符串, 代表要执行的语句. 
该语句受后面两个字典类型参数\ ``globals``\ 和\ ``locals``\ 的限制, 只有在\ ``globals``\ 字典和\ ``locals``\ 字典作用域范围内的函数和变量才能被执行;

*   ``globals``

这个参数管控的是一个全局的命名空间, 即\ ``expression``\ 可以使用全局命名空间中的函数. 
如果只是提供了\ ``globals``\ 参数, 而没有提供自定义的\ ``__builtins__``\ , 则系统会将当前环境中的\ ``__builtins__``\ 复制到自己提供的\ ``globals``\ 中, 
然后才会进行计算; 如果连\ ``globals``\ 这个参数都没有提供, 则使用Python的全局命名空间.

*   ``locals``

这个参数管控的是一个局部的命名空间, 和\ ``globals``\ 类似, 当它和\ ``globals``\ 中有重复或冲突时, 以\ ``locals``\ 的为准. 
如果\ ``locals``\ 没有被提供, 则默认为\ ``globals``.


.. note::

    ``__builtin__``\ 是Python的内建模块.

``globals``\ 参数使用示例:

.. code-block:: python

    dic = {} # 定义一个空字典
    dic['b'] = 10 # 在dic中添加一个元素, key为b
    print(dic.keys()) # 先将dic的key打印出来, 有一个元素b
    exec('a = 20', dic} # 在exec执行的语句后面跟一个作用域dic
    print(dic.keys()) # 执行exec后, dic的key多了一个

    运行结果为:
    dict_keys(['b'])
    dict_keys(['b', '__builtins__', 'a'])

上面的代码是在作用域dic下执行了一句\ ``a = 4``\ 的代码.
可以看出, ``exec()``\ 之前dic的key中只有一个b. 执行完\ ``exec()``\ 之后, 系统在dic中生成了两个新的key, 分别是\ ``a``\ 和\ ``__builtins__``\ .
其中, ``a``\ 为执行语句生成的变量, 系统将其放到指定的作用域字典里; ``__builtin__``\ 是系统加入的内置key.

``locals``\ 参数使用示例:

.. code-block:: python

    a = 10
    b = 20
    c = 30
    d = 200
    g = {'a': 40, 'b': 50}
    l = {'b': 60, 'c': 70}
    print(eval('a + b + c', g, l))

    运行结果为:
    170


``eval()``\ 和\ ``exec()``\ 的区别
----------------------------------

它们的区别在于, ``eval()``\ 执行完会返回结果, ``exec()``\ 执行完不返回结果.

Example:

.. code-block:: python

    a = 1
    exec('a = 2') # 相当于执行a = 2
    print(a)
    a = exec('2 + 10') # 相当于执行2 + 10, 但是没有返回值, a应该为None
    print(a)
    a = eval('2 + 10') # 执行2 + 10, 并把结果返回给a
    print(a)

    运行结果为:
    2
    None
    12

如果在\ ``eval()``\ 里放置了一个没有结果返回的语句, 则Python解释器会报\ ``SyntaxError``\ 错误.

Example:

.. code-block:: python

    a = eval('a = 200')

    运行结果为:
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "<string>", line 1
        a = 200
          ^
    SyntaxError: invalid syntax


注意事项
--------

使用\ ``eval()``\ 和\ ``exec()``\ 函数时, 一定要记住, 它们的第一个参数是字符串, 而字符串的内容一定要是可执行的代码.

以\ ``eval()``\ 函数为例, 用代码演示常犯的错误:

.. code-block:: python

    s = 'hello'
    print(eval(s))

运行结果如下:

.. code-block:: text

    Traceback (most recent call last):
    File "./demo.py", line 4, in <module>
        print(eval(s))
    File "<string>", line 1, in <module>
    NameError: name 'hello' is not defined

上面例子出错的地方在于, 字符串的内容是hello, 而hello并不是可执行的代码(除非定义了变量叫做hello).

如果要将字符串hello通过print函数打印出来, 可以写成如下的样子:

.. code-block:: python

    s = 'hello'
    print(eval('s'))

    运行结果为:
    hello

这种写法是要\ ``eval()``\ 执行\ ``'hello'``\ 这句代码. 
这个hello是有引号的, 在代码中代表字符串的意思, 所以可以执行.

同理, 也可以写成这样:

.. code-block:: python

    s = '"hello"'
    print(eval(s))

    运行结果为:
    hello

这种写法的意思是s是个字符串, 并且其内容是个带引号的hello, 所以直接将s放入到函数\ ``eval()``\ 中也可以执行.

除了以上几种方式, 还可以不去改变原有字符串s的写法, 直接使用\ ``repr()``\ 函数来进行转化, 也可以得到同样的效果.

Example:

.. code-block:: python

    s = 'hello'
    print(eval(repr(s)))

    运行结果为:
    hello

注意, 虽然函数\ ``repr()``\ 和\ ``str()``\ 的返回值都是字符串, 但是使用\ ``str()``\ 函数对s进行转换, 程序同样会报错:

.. code-block:: python

    s = 'hello'
    print(eval(str(s)))

运行结果为:

.. code-block:: text

    Traceback (most recent call last):
    File "./demo.py", line 4, in <module>
        print(eval(str(s)))
    File "<string>", line 1, in <module>
    NameError: name 'hello' is not defined

为什么会有这个区别呢?
同样对字符串s的转化, 使用\ ``repr()``\ 和\ ``str()``\ 得到的结果是有差别的, 直接将两者的结果打印出来, 就可以明显看出不同.

.. code-block:: python

    s = 'hello'
    print(repr(s))
    print(str(s))

    运行结果为:
    'hello'
    hello

可见, 使用\ ``repr()``\ 返回的内容, 输出后会在两边多一个单引号.

.. note::

    在编写代码时, 一般会使用\ ``repr()``\ 函数来生成动态地字符串, 再传入到\ ``eval()``\ 或\ ``exec()``\ 函数内, 实现动态执行代码的功能.

