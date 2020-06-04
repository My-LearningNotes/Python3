Python复数类型
==============

**复数(Complex)**\ 是Python的内置类型, 直接书写即可. 
换句话说, Python语言本身就支持复数, 而不依赖标准库或第三方库.

复数由实部(real)和虚部(imag)构成, 复数的虚部以\ ``j``\ 或者\ ``J``\ 作为后缀, 具体格式为:

.. code-block:: python

    a + bj

``a``\ 表示实部, \ ``b``\ 表示虚部.

Example:

.. code-block:: python

    c1 = 12 + 0.2j
    print(c1)
    print(type(c1))

