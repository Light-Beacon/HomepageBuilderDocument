工程
=================
工程是构建器的核心概念，它包含了所有的配置、模板和脚本等内容。每个工程都是一个独立的目录，包含了构建器所需的所有文件。

创建工程
-----------------
要创建一个新的工程，你可以在空目录下使用以下命令：

.. code-block:: shell

    builder create

.. warning:: 使用命令时请确保当前目录为空，否则可能会破坏现有文件。若目录不为空在运行命令时也会提示你确认是否确认继续创建。

工程目录结构
-----------------
基本结构
~~~~~~~~~~~~~~~~~
基本结构是构建器进行构建必不缺少的目录结构。一个基本的工程目录结构如下：
.. code-block:: 

    project/
    ├── libraries/
    │   ├── __LIBRARY__.yml
    │   ├── card1.yml
    │   └── card2.yml
    ├── pages/
    │   ├── page1.yml
    │   └── page2.yml
    └── project.yml

- **project.yml**: 工程的描述文件，包含工程的基本信息和配置。 
- **libraries/**: 存放卡片库文件。
- **pages/**: 存放工程的页面文件。

完整结构
~~~~~~~~~~~~~~~~~
除了基本结构外，构建器还支持更多的目录和文件来组织工程内容。完整的工程目录结构包含了配置、数据、资源等多个部分，以便于管理和扩展。

工程的完整目录结构示例如下：

.. code-block:: 

    project/
    ├── config/
    │   └── config.yml
    ├── data/
    │   └── data.json
    ├── libraries/
    │   ├── __LIBRARY__.yml
    │   └── LibraryA/
    │       ├── LibraryB/
    │       │   └── card2.yml
    │       └── card1.yml
    ├── modules/
    │   └── module.py
    ├── pages/
    │   ├── page1.yml
    │   └── page2.yml
    ├── resources/
    │   ├── style1.yml
    │   ├── style2.xml
    │   ├── strings.xaml
    │   └── control_template.xaml
    ├── structures/
    │   ├── components/
    │   │   ├── component1.yaml
    │   │   └── component2.yaml
    │   └── templates/
    │       ├── template1.yaml
    │       └── template2.yaml
    └── project.yml

- **config/**: 存放工程的配置文件。
- **data/**: 存放工程使用的数据文件。
- **modules/**: 存放工程的模块文件。
- **resources/**: 存放工程的资源文件。
- **structures/**: 存放工程的结构文件。

  - **components/**: 存放组件文件。
  - **templates/**: 存放模板文件。
  
工程描述文件
-----------------
工程描述文件 `Project.yml` 是工程的核心配置文件，定位了工程文件夹，内容包括工程适用的构建器版本以及默认页面。

工程描述文件的基本结构如下：

.. code-block:: yaml

    version: 0.14.5
    default_page: News

- **version**: 构建器的版本号，指定该工程适用的构建器版本。
- **default_page**: 默认页面的名称，当未指定页面时，构建器会使用该页面作为默认页面。