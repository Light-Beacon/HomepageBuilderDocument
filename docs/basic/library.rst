卡片库
===================
卡片库是一个用于储存和管理卡片的地方。所有卡片都存储在卡片库中。

卡片库存放于工程文件夹下的 `libraries` 文件夹中。

目录结构
-------------------
卡片库的目录结构如下：

.. code-block:: 

    libraries/
    ├── __LIBRARY__.yml
    ├── LibraryA/
    │   ├── LibraryB/
    │   │   ├── __LIBRARY__.yml
    │   │   └── card2.md
    │   ├── __LIBRARY__.yml
    │   └── card1.md
    ├── card3.md
    └── card4.xaml

其中 `__LIBRARY__.yml` 是卡片库的描述文件，包含了卡片库的基本信息和配置。
卡片库可以包含多个子库，每个子库也必须有自己的描述文件。

除了 `__LIBRARY__.yml` 文件外，卡片库还包含其他类型的文件，例如 Markdown 文件、XAML 文件等，一个文件就是一个卡片。

卡片库描述文件
---------------------
下面是一个描述文件的示例：

.. code:: yaml

    name: library_name
    indexing: public
    default:
        title: Default_Title
        templates:
            - MarkdownCard
            - Raw
    override:
        canswap: false

**name**: 卡片库的唯一标识名
**indexing**: 卡片库的索引策略
**default**: 卡片的默认属性
**override**: 覆盖卡片的属性

卡片
------------------
卡片是卡片堆叠页面进行构建的基本单元。
本质上卡片是一个属性集合，当需要构建卡片时构建器会根据卡牌的`template`属性选择模版进行构建。（详见模版页面）

卡片的标识名是去除后缀的文件名，例如 `card1.md` 的标识名为 `card1`。

卡片属性
~~~~~~~~~~~~~~~~~~
卡片属性有以下几种写入方式：

1. 卡片库提供
2. 卡片的模版提供
3. 卡片的文件处理函数提供属性
4. 页面指定

卡片库和卡片模版中可以有 `default` 和 `override` 字段。

`default` 字段提供了库(或模版)中的卡片的默认属性。`override` 字段覆盖了卡片的默认属性（即使没有）。

例如如下目录结构：

.. code-block:: 

    libraries/
    ├── __LIBRARY__.yml
    ├── LibraryA/
    │   ├── __LIBRARY__.yml
    │   └── card2.md
    └── card1.md

其中 `libraries` 中的 `__LIBRARY__.yml` 文件内容如下：

.. code:: yaml

    ...
    default:
        title: Default_Title
    override:
        canswap: false
    ...

所以现在该卡片库中的卡片(card1.md)现在包含两个属性：

.. code:: yaml

    title: Default_Title
    canswap: false

继承与重载
++++++++++++++++++++++

上面两个属性除了给库里的卡片设置属性，还向其子库的卡片提供属性，所以如果子库的描述文件没有修改这些属性，它们将自动继承这些值。

如果需要修改子库中的卡片属性，可以在子库的描述文件中override这些属性。

若 `LibraryA` 中的 `__LIBRARY__.yml` 文件内容如下：

.. code:: yaml

    ...
    default:
        canswap: false
    override:
        title: Overrided_Title
    ...

那么 `LibraryA` 中的 `card2.md` 文件将包含以下属性：

.. code:: yaml

    title: Overrided_Title
    canswap: false

属性设置的优先级如下（从高到低）：

1. 子模版override
2. 父模版override
3. 页面指定属性
4. 父库override
5. 子库override
6. 卡片文件处理函数
7. 子库default
8. 父库default
9. 父模版default
10. 子模版default

卡片引用与索引
~~~~~~~~~~~~~~~~~~
当页面指定要找到某一个卡片时，会使用卡片引用让构建器查找卡片。完整的卡片引用应该格式如下：

.. code::

    RootLibrary:AncestorLibrary:...:ChildrenLibrary:Card

在最后一个冒号前的都是卡片库的路径，从根卡片到卡片的最上层库，用分号分割的库唯一标识名。

但这样做非常麻烦，于是构建器提供了卡片索引，可以省略或简化库路径，以便于简化卡片引用。

库路径只需要符合前后关系即可，所以上述卡片引用可简化为以下形式的任意一种：

- Card
- RootLibrary:Card
- RootLibrary:AncestorLibrary:Card
- RootLibrary:ChildrenLibrary:Card
- AncestorLibrary:ChildrenLibrary:Card
- ChildrenLibrary:Card

一般只有在有卡片重名的情况会需要指定库路径。

索引策略
++++++++++++++++
库通过索引策略可以选择是否将自己以及自己的卡片以及子库添加进索引。
有以下五个可能的值：

- **public**：正常加入上一级索引（默认）
- **protect_sub**：子库不加入上一级索引
- **protect_file**：库内文件（卡片）不加入上一级索引
- **private**：子库与库内文件均不加入上一级索引
- **hidden**：库本身以及所有子内容均不加入索引