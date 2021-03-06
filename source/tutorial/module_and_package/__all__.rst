``__all__``\ 变量
=================

事实上, 当我们以\ ``from module import *``\ 的形式导入某个模块中的成员时, 导入的是该模块中那些名称不以下划线(单下划线\ ``_``\ 或者双下划线\ ``__``\)开头的变量, 函数和类. 

可以在模块中定义\ ``__all__``\ 变量, 该变量的值是一个列表, 存储的是当前模块中一些成员(变量, 函数或类)的名称. 
通过在模块文件中定义\ ``__all__``\ 变量, 当其它文件以\ ``from module import *``\ 的形式导入该模块时, 只会导入\ ``__all__``\ 列表中定义的成员.

.. attention::

    ``__all__``\ 变量的作用仅限于在其它文件中以\ ``from module import *``\ 的形式导入时.

