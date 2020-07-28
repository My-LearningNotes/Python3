字符串对齐
==========

Python字符串提供了3种可以用来进行文本对齐的方法, 分别是\ ``ljust()``\, ``rjust()``\ 和\ ``center()``\ 方法.


``ljust()``\ 方法
-----------------

``ljust()``\ 方法的功能是向指定字符串的右侧填充指定字符, 从而达到左对齐文本的目的.

.. code-block:: python

    string.ljust(width[, fillchar])

*   ``width``\ 表示包括字符串本身在内, 字符串要占的总长度;
*   ``fillchar``\ 作为可选参数, 用来指定填充字符串时所用的字符, 默认情况使用空格.


``rjust()``\ 方法
-----------------

``rjust()``\ 和\ ``ljust()``\ 方法类似, 唯一的不同在于, ``rjust()``\ 方法是向字符串的左侧填充指定字符, 从而达到右对齐的目的.

.. code-block:: python

    string.rjust(width[, fillchar])


``center()``\ 方法
------------------

``center()``\ 字符串方法与\ ``ljust()``\ 和\ ``rjust()``\ 的用法类似, 但它让文本居中, 而不是左对齐或有对齐.

.. code-block:: python

    string.center(width[, fillchar])

