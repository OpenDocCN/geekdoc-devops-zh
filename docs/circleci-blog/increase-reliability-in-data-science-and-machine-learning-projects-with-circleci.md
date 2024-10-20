# 提高数据科学和机器学习项目的可靠性

> 原文：<https://circleci.com/blog/increase-reliability-in-data-science-and-machine-learning-projects-with-circleci/>

数据科学不再是公司的小众话题。从首席执行官到实习生，每个人都知道采取科学方法处理数据是多么有价值。因此，许多不直接从事软件工程领域的人开始编写更多的代码，通常是以交互式笔记本的形式，如 Jupyter。软件工程师通常是构建系统、代码静态分析和生成可重复过程以加强质量的巨大倡导者。写代码 Jupyter 笔记本的商务人士呢？他们可以使用哪些流程来使他们的数据科学、机器学习和 AI 代码更加可靠？

## 数据科学项目质量

提高数据科学软件质量的一个方法是创建一个确保质量和可重复性的项目结构。要做到这一点，可以从传统的软件工程世界中汲取一些思想。Brian Kernigan 是《AWK 编程语言》和《K 和 R C》的合著者，他在《软件工具》一书中总结了软件开发的真正本质，他说:“控制复杂性是软件开发的本质。”

在我以前写的一篇关于代码质量的文章中，关于 Python 中的软件工程项目质量，我说过如下的话:

“编写高质量代码的第一步是重新审视个人或团队如何开发软件的整个思维过程。通常，在失败的或有问题的软件开发项目中，软件是在一种反动的意识流中开发的，软件开发的焦点是以任何可能的方式解决问题。在一个成功的软件项目中，开发人员不仅要考虑如何解决手头的问题，还要考虑解决问题的过程。

一个成功的软件开发人员会设计出一种容易自动化的方式来运行测试，这样他们就可以不断地证明软件是有效的。他们意识到不必要的复杂性的危险。他们态度谦逊，寻求批判性的评论，并期望在每一步都进行重构。他们不断地思考如何确保他们的软件是可测试的、可读的和可维护的。"

数据科学项目也是如此；需要一种自动化的方法来确保质量。幸运的是，有了 CircleCI 和开源库这样的服务，这很容易实现。在下面的章节中，我们将一步一步地演示这一点。

## 数据科学项目自动化测试设置

为数据科学项目建立适当的自动化测试的最好方法之一是从一开始就正确地设置它。那看起来像什么？

*   创建一个 GitHub 项目。创建一个像[这样的新项目](https://github.com/noahgift/myrepo)示例回购。
*   创建一个包含一个`config.yml`文件的`.circleci`目录。[这个](https://github.com/noahgift/myrepo/blob/master/.circleci/config.yml)是你可以参考的`config.yml`的例子。
*   创建一个`.gitignore`文件。忽略不重要的文件是很重要的。
*   创建一个`README.md`文件。一个好的`README.md`应该能够展示用户如何构建项目以及项目做什么。包含一个显示 CircleCI 构建状态的徽章也很有帮助，比如[这个](https://circleci.com/gh/noahgift/myrepo.svg?style=svg)例子。
*   创建一个`Makefile`。一个`Makefile`是一种在构建过程中运行步骤的常见方式，已经存在了几十年…因为某种原因…它们只是工作。我们将介绍如何为数据科学项目设置这一点。
*   其他可选的重要文件和目录有:库目录、命令行工具、requirements.txt 和测试目录。

一个很好的起点是将一个`Makefile`作为模板。`myrepo/Master/Makefile`的内容如下所示，可以在这里找到[:](https://raw.githubusercontent.com/noahgift/myrepo/master/Makefile)

```
setup:
	python3 -m venv ~/.myrepo

install:
	pip install -r requirements.txt

test:
	python -m pytest -vv --cov=myrepolib tests/*.py
	python -m pytest --nbval notebook.ipynb

lint:
	pylint --disable=R,C myrepolib cli web

all: install lint test 
```

关键步骤有:`setup`、`install`、`test`、`lint`、`all`(运行一切)。设置步骤会创建一个可选的虚拟环境，稍后可以通过运行以下命令来获得该环境:

```
source ~/.myrepo/bin/activate` 
```

可以作为`make install`运行的`install`步骤安装 requirements.txt 文件中列出的包。这里的就是一个例子[。](https://github.com/noahgift/myrepo/blob/master/requirements.txt)

如果创建了库、命令行工具或 web 应用程序，那么`lint`步骤是有意义的，可以使用`make install`运行。仅仅在 Jupyter 笔记本上运行是没有意义的，但是它确实有助于保持与项目相关的代码的质量。下面是 lint 的输出示例，也可以在[这里](https://gist.github.com/noahgift/62931536a2ceaf3f23e4e611d4fa6574)找到:

```
(.myrepo) ➜  myrepo git:(master) ✗ make lint
pylint --disable=R,C myrepolib cli web
No config file found, using default configuration

--------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 10.00/10, +0.00) 
```

最后也是最重要的一步是运行`make test`。这使用了`pytest`和 [`nbval`插件](https://github.com/computationalmodelling/nbval)。输出如下所示，也可以在[这里](https://gist.github.com/noahgift/c82dc69f8827166b42df6342970c77b2)找到。

```
(.myrepo) ➜  myrepo git:(master) ✗ make test
python -m pytest -vv --cov=myrepolib tests/*.py
============================================================ test session starts ============================================================
platform darwin -- Python 3.6.4, pytest-3.3.0, py-1.5.2, pluggy-0.6.0 -- /Users/noahgift/.myrepo/bin/python
cachedir: .cache
rootdir: /Users/noahgift/src/myrepo, inifile:
plugins: cov-2.5.1, nbval-0.7
collected 1 item                                                                                                                            

tests/test_myrepo.py::test_func PASSED                                                                                                [100%]

---------- coverage: platform darwin, python 3.6.4-final-0 -----------
Name                    Stmts   Miss  Cover
-------------------------------------------
myrepolib/__init__.py       1      0   100%
myrepolib/repomod.py       11      4    64%
-------------------------------------------
TOTAL                      12      4    67%

========================================================= 1 passed in 0.02 seconds ==========================================================
python -m pytest --nbval notebook.ipynb
============================================================ test session starts ============================================================
platform darwin -- Python 3.6.4, pytest-3.3.0, py-1.5.2, pluggy-0.6.0
rootdir: /Users/noahgift/src/myrepo, inifile:
plugins: cov-2.5.1, nbval-0.7
collected 4 items                                                                                                                           

notebook.ipynb ....                                                                                                                                 [100%]

===================================================================== warnings summary ======================================================================
notebook.ipynb::Cell 0
  /Users/noahgift/.myrepo/lib/python3.6/site-packages/jupyter_client/connect.py:157: RuntimeWarning: Failed to set sticky bit on '/var/folders/vl/sskrtrf17nz4nww5zr1b64980000gn/T': [Errno 1] Operation not permitted: '/var/folders/vl/sskrtrf17nz4nww5zr1b64980000gn/T'
    RuntimeWarning,

-- Docs: http://doc.pytest.org/en/latest/warnings.html
=========================================================== 4 passed, 1 warnings in 2.08 seconds ============================================================ 
```

有必要解释一下`nbval`插件是如何工作的。简而言之，它为您运行 Jupyter 笔记本，并确保所有的单元格都执行。有两种模式可以使用:一种实际检查每个单元的输出，另一种不检查。检查每个单元的输出的方法可能有点棘手，因为很多时候随机图像或输出都在单元中，并且测试将在每次后续运行中失败。

所有这些都解决了之后，让 CircleCI 运行起来就没什么可做的了。该信息包含在 CircleCI [文档](https://circleci.com/docs/language-python/)中。圣代的最后一个收获是让徽章发挥作用，这也包含在官方文件[中](https://circleci.com/docs/status-badges/)，在本文分享的[回购](https://github.com/noahgift/myrepo)中有一个例子。

## 摘要

本文展示了如何引导一个数据科学项目，设置 Github 结构，运行测试，然后将其发送到 CircleCI 进行构建。我在 YouTube 上制作了一个视频，展示了如何安装和测试这个项目。另一个值得关注的资源是阅读《实用人工智能:基于云的机器学习导论》一书，了解我是如何使用 CircleCI 的。参考资料中有相关链接。

Noah Gift 是加州大学戴维斯分校管理研究生院 MSBA 项目和 MSDS 西北大学研究生数据科学项目的讲师和顾问，他在那里教授和设计研究生机器学习、人工智能和数据科学课程，并为学生和教师提供机器学习和云架构方面的咨询。他已经发表了近 100 篇技术出版物，包括两本关于从云机器学习到 DevOps 等主题的书籍。他最近的一本书是《实用人工智能:基于云的机器学习导论》(Pearson，2018)。

## 参考