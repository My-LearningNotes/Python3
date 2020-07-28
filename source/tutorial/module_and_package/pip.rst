Python包管理工具
================

使用Python进行程序开发时, 除了使用Python内置的标准模块以及自定义的模块之外, 还有很多第三方模块可以使用, 这些第三方模块可以在Python官方提供的网站(https://pypi.org/)上查找.

在使用第三方模块之前, 需要先下载并安装该模块, 然后就能像标准模块和自定义模块一样导入并使用了.

``pip``\ 是Python的包管理工具, 该工具提供了对Python包的查找, 下载, 安装, 卸载等功能.
在安装Python之后, 包管理工具也就自动安装了.

``pip``\ 和\ ``pip3``
---------------------

一般情况下, Python 2.x使用\ ``pip``\ ; Python 3.x使用\ ``pip3``\ . 

``pip``\ 和\ ``pip3``\ 会将包安装到不同的路径下. 
``pip``\ 会将包安装到Python 2.x默认的安装路径下, 所以在Python 2.x中可以直接导入; 
``pip3``\ 会将包安装到Python 3.x默认的安装路径下, 所以在Python 3.x中可以直接导入.


常用指令
--------

*   显示版本和路径

.. code-block:: python

    pip3 --version

*   显示帮助信息

.. code-block:: python

    pip3 --help

*   搜索

.. code-block:: python

    pip3 search somepackage

*   安装

.. code-block:: python

    # 最新版本
    pip3 install somepackage

    # 指定版本
    pip3 install somepackage=x.x.x

    # 最低版本
    pip3 install somepackage>x.x.x

*   升级

.. code-block:: python

    pip3 insall -U/--upgrade somepackage

*   卸载

.. code-block:: python

    pip3 uninstall somepackage
    pip3 uninstall -r <requirements files>

*   列出已安装的包

.. code-block:: python

    pip3 list

*   显示已安装的包的信息

.. code-block:: python

    pip3 show somepackage


使用国内的源
------------

用\ ``pip``\ 安装包时, 默认使用国外的源, 因为网络问题, 安装通常很慢. 
为此, 可以使用一些国内的镜像替换\ ``pip``\ 源.

比较常用的国内镜像有:

    *   阿里云 http://mirrors.aliyun.com/pypi/simple/
    *   豆瓣 http://pypi.douban.com/simple/
    *   清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
    *   中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
    *   华中科技大学 http://pypi.hustunique.com/

.. attention::

    新版的Ubuntu要求使用https源.

*   **临时使用国内的源**

如果只是临时使用国内的源, 可以使用\ ``-i``\ 参数来指定国内的源.

Example:

.. code-block:: python

    # 以清华源为例
    # xxx为要安装的包
    pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple xxx

*   **配置国内源为默认使用**

    -   Linux下

    创建\ ``~/.pip/pip.conf``\ 文件, 在其中添加如下内容:

    .. code-block:: python

        [global]
        index-url = https://pypi.tuna.tsinghua.edu.cn/simple

        [install]
        trusted-host = https://pypi.tuna.tsinghua.edu.cn

    之后安装包时, 默认就是使用国内的源.

.. note::

    对于\ ``pip3``\ , 也是创建并配置\ ``~/.pip/pip.conf``\ 文件来使用国内的镜像.

