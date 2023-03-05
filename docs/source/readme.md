# Sphinx项目部署到Github Pages

## 创建Sphinx项目

[官方入门](https://www.sphinx-doc.org/en/master/tutorial/getting-started.html)

[.rst语法](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#toctree-directive)

1. 安装python环境和Sphinx包。

   ```
   python -m pip install sphinx
   ```

 2. 如果你正确安装了sphinx包，输入以下指令，将可以看到sphinx的版本号。

    ```
    sphinx-build --version
    ```

 3. 然后创建文档布局。

    ```
    sphinx-quickstart docs
    ```

    命令成功后，会要求输入一些信息：

    ```
    > Separate source and build directories (y/n) [n]：输入“ y”（不带引号）并按Enter。
    > Project name：输入“ Lumache”（不带引号）并按 Enter。
    > Author name(s)：输入“ Graziella”（不带引号）并按 Enter。
    > Project release []：输入“ 0.1”（不带引号）并按 Enter。
    > Project language [en]：将其留空（默认为英语）并按 Enter。"zh_CN"为中文。
    ```

    按照上述提示输入信息后，你将得到这样一个目录结构：

    ```
    docs
    ├── build
    ├── make.bat
    ├── Makefile
    └── source
       ├── conf.py
       ├── index.rst
       ├── _static
       └── _templates
    ```

## 支持md文件

pip安装插件以支持md格式文件：

```
pip install sphinx_rtd_theme      //蓝色的主题
pip install recommonmark
pip install sphinx_markdown_tables
```

修改目录下"conf.py"文件，以支持插件：

```
extensions = ["sphinx.ext.duration","recommonmark","sphinx_markdown_tables"]
```

更改主题（可选）：

```
html_theme = 'sphinx_rtd_theme' 
```

更改Logo（可选）：

```
html_logo = './_static/logo.jpg'
```

## 添加md文件

修改 “index.rst” 文件，添加 “readme.md” 文件：

```
.. notes documentation master file, created by
   sphinx-quickstart on Sun Mar  5 06:32:01 2023.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to notes's documentation!
=================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:
   
   readme.md



Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```

## 运行构建html

使用以下指令构建：

```
sphinx-build -b html sourcedir builddir
sourcedir：指定源文件目录；
builddir：制定生成目录，这里由于github主页限制，必须制定"docs"文件夹；
```

也可简单构建，使用自带的 make.bat 工具构建 html 页面：

```
make html
```

## 上传github仓库

作为 github.io 主页上传。

修改 branch 和主页根目录为 ”_build/html” 目录。

[actions工作流参考](https://coderefinery.github.io/documentation/gh_workflow/)

## 解决加载不了 css、logo 的路径问题

.nojekyll

