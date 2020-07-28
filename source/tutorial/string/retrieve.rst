检索字符串
==========


``find()``\ 方法
----------------

``find()``\ 方法用于检索字符串中是否包含目标字符, 如果包含, 则返回第一次出现该字符串的索引, 否则返回-1.

.. code-block:: python

    string.find(sub[, start[, end]])

*   ``sub``\ 表示要检索的目标字符串;
*   ``start``\ 表示开始检索的位置, 如果不指定, 则默认从头开始检索;
*   ``end``\ 表示结束检索的位置, 如果不指定, 则默认一直检索到结尾.

注意, Python还提供了\ ``rfind()``\ 方法, 于\ ``find()``\ 方法最大的不同在于, ``rfind()``\ 是从字符串右边开始检索.


``index()``\ 方法
-----------------

同\ ``find()``\ 方法类似, ``index()``\ 方法也可以用于检索是否包含指定的字符串, 不同之处在于, 当指定的字符串不存在时, ``index()``\ 方法会抛出异常.

.. code-block:: python

    string.index(sub[, start[, end]])

同\ ``find()``\ 和\ ``rfind()``\ 一样, 字符串还有\ ``rindex()``\ 方法, 其作用和\ ``index()``\ 方法类似, 不同之处在于它是从右边开始检索.


``startswith()``\ 方法
----------------------

``startswith()``\ 方法用于检索字符串是否以指定字符串开头, 如果是返回\ ``True``\; 反之返回\ ``False``\ .

.. code-block:: python

    string.startswith(sub[, start[, end]])


``enswith()``\ 方法
-------------------

``endswith()``\ 方法用于检索字符串是否以指定字符串结尾, 如果是则返回\ ``True``\; 反之则返回\ ``False``\ .

.. code-block:: python

    string.endswith(sub[, start[, end]])

