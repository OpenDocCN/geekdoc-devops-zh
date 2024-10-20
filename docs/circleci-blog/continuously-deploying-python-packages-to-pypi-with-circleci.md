# 使用 CircleCI - CircleCI 将 Python 包持续部署到 PyPI

> 原文：<https://circleci.com/blog/continuously-deploying-python-packages-to-pypi-with-circleci/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

Python 包索引通常被称为 PyPI，是 Python 编程语言的软件仓库。每次运行`pip install $PACKAGE`时，你都在使用 PyPI。在这篇文章中，您将学习如何使用 git 标签和 CircleCI 将您自己的 Python 包持续部署到 PyPI。

在过去的几周里，我一直在为 CircleCI API 开发一个 [Python 包装器。这个项目使用了我们将要在这里讨论的同样的方法。](https://github.com/levlaz/circleci.py)

## 属国

这种方法在标准 Python 库之外唯一需要的依赖项是 [twine](https://pypi.python.org/pypi/twine) 包。

## 假设

本教程假设以下情况属实:

1.  你在 PyPI 上有账户。如果没有，很容易上手。可以[在这里](https://pypi.python.org/pypi?%3Aaction=register_form)注册。
2.  您的项目设置中有一个名为`PYPI_PASSWORD`的环境变量，它引用您的 PyPI 密码。
3.  您有一个遵循标准打包指南的 Python 包。如果没有，Python 提供了一些关于如何[打包和分发项目](https://packaging.python.org/en/latest/tutorials/packaging-projects/)的优秀文档。
4.  您正在使用 [git 标签](https://git-scm.com/book/en/v2/Git-Basics-Tagging)来创建一个发布。
5.  您正在使用带有[工作流](https://circleci.com/docs/workflows/)的 CircleCI 2.0。

## 发布工作流

下面描述了高级发布工作流。

1.  一旦您准备好剪切项目的新版本，您就可以在`setup.py`中更新版本，并使用`git tag $VERSION`创建一个新的 git 标签。
2.  一旦用`git push --tags`将标签推送到 GitHub，就会触发一个新的 CircleCI 构建。
3.  您运行一个验证步骤来确保 git 标记与您在上面的步骤 1 中添加的 my project 的版本相匹配。
4.  CircleCI 执行你所有的测试(你有测试对不对？).
5.  一旦所有测试都通过，您就可以创建一个新的 Python 包，并使用 [twine](https://pypi.python.org/pypi/twine) 将其上传到 PyPI。

## 完整演练

有了所有这些假设，并在头脑中有了一个高层次的概述，我们就可以开始研究真实世界的例子了。

### setup.py

[setup.py](https://packaging.python.org/tutorials/distributing-packages/#setup-py) 文件是任何 Python 包中最重要的部分。它为 PyPI 提供元数据，处理所有打包任务，甚至允许您添加定制的打包相关命令。我们的示例项目中的文件如下所示:

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-
"""
:copyright: (c) 2017 by Lev Lazinskiy
:license: MIT, see LICENSE for more details.
"""
import os
import sys

from setuptools import setup
from setuptools.command.install import install

# circleci.py version
VERSION = "1.1.1"

def readme():
    """print long description"""
    with open('README.rst') as f:
        return f.read()

class VerifyVersionCommand(install):
    """Custom command to verify that the git tag matches our version"""
    description = 'verify that the git tag matches our version'

    def run(self):
        tag = os.getenv('CIRCLE_TAG')

        if tag != VERSION:
            info = "Git tag: {0} does not match the version of this app: {1}".format(
                tag, VERSION
            )
            sys.exit(info)

setup(
    name="circleci",
    version=VERSION,
    description="Python wrapper for the CircleCI API",
    long_description=readme(),
    url="https://github.com/levlaz/circleci.py",
    author="Lev Lazinskiy",
    author_email="lev@levlaz.org",
    license="MIT",
    classifiers=[
        "Development Status :: 5 - Production/Stable",
        "Intended Audience :: Developers",
        "Intended Audience :: System Administrators",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
        "Topic :: Software Development :: Build Tools",
        "Topic :: Software Development :: Libraries :: Python Modules",
        "Topic :: Internet",
        "Programming Language :: Python :: 3",
        "Programming Language :: Python :: 3.6",
        "Programming Language :: Python :: 3 :: Only",
    ],
    keywords='circleci ci cd api sdk',
    packages=['circleci'],
    install_requires=[
        'requests==2.18.4',
    ],
    python_requires='>=3',
    cmdclass={
        'verify': VerifyVersionCommand,
    }
) 
```

有几点需要注意:

1.  `VERSION`被设置为文件顶部的常数。这应该总是与您最新的 git 标签相匹配。
2.  `setup()`函数相当标准，提供了 PyPI 需要的所有必要的元数据。
3.  我们有一个名为`verify`的定制命令，它将确保我们的 git 标签与我们的`VERSION`相匹配。我们将此命令作为工作流程的一部分来运行，以确保我们发布的是正确的版本。

这个项目的 CircleCI 配置文件中有趣的部分如下所示。你可以在这里看到完整的文件。

#### 部署作业

一旦我们安装了所有的项目依赖项，为打包做好准备，我们就运行定制的`verify`命令(在上一节中讨论过)来确保 git 标签与我们即将发布的版本相匹配。

```
 - run:
          name: verify git tag vs. version
          command: |
            python3 -m venv venv
            . venv/bin/activate
            python setup.py verify 
```

接下来，我们使用在项目设置中设置的`PYPI_PASSWORD`环境变量创建一个`.pypirc`文件。

```
 - run:
          name: init .pypirc
          command: |
            echo -e "[pypi]" >> ~/.pypirc
            echo -e "username = levlaz" >> ~/.pypirc
            echo -e "password = $PYPI_PASSWORD" >> ~/.pypirc 
```

然后我们创建所有的包。

```
 - run:
          name: create packages
          command: |
            make package 
```

为了方便起见，这个项目使用了一个 Makefile 文件。如果您不想使用 Makefile，您可以在本节中手动运行命令。

创建包的命令有:

```
# create a source distribution
python setup.py sdist

# create a wheel
python setup.py bdist_wheel 
```

最后，我们使用 twine 将刚刚创建的包上传到 PyPI。

```
 - run:
          name: upload to pypi
          command: |
            . venv/bin/activate
            twine upload dist/* 
```

我们配置的部署作业只有在构建是 git 标记时才被触发，并且依赖于我们测试作业的通过。此类设置的配置如下所示:

```
workflows:
  version: 2
  build_and_deploy:
    jobs:
      - build:
          filters:
            tags:
              only: /.*/
      - deploy:
          requires:
            - build
          filters:
            tags:
              only: /[0-9]+(\.[0-9]+)*/
            branches:
              ignore: /.*/ 
```

## 摘要

就是这样！现在，每当你为你的项目推一个新的 git 标签时，你将自动创建一个新的包，并把它上传到 PyPI。这允许您拥有一个完全独立的、可重复的连续部署管道。最重要的是，通过使用测试、持续集成和持续部署，您能够确保您的包的用户在运行`pip install $YOUR_PACKAGE`时只获得最高质量的包。