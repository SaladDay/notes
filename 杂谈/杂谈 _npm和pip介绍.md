---
title: npm和pip介绍
date: 2021-11-11 15:52:41
tags: 
categories: 杂谈
cover:
---

> 相信大家应该经常看到 npm 和 pip 两个术语，我也经常看到，pip 经常用，但是 npm 只是眼熟，没仔细了解过，今天了解 typescript 的时候又看到了，所以特地对比了一下 pip 和 npm 两个，查阅相关资料和博客后，发现 npm 和 pip 简直出奇的像，功能非常类似，都是负责各自领域的工具包的安装和下载，只不过 npm 服务于 Node.js，pip 服务于 Python。甚至可以共用一套相同的文字介绍框架，仅仅是替换了少数术语名词。

### pip

- pip，全称是：package installer for Python，它是一个现代的，通用的 Python 包管理工具，是 easy_install 的替代品。提供了对 Python 包的查找、下载、安装、卸载的功能。只要有人把某个代码模块打包放在一个叫做 PiPI 的地方，其他人就可以从这个 PiPI 中下载安装包。只要包存在于 PiPI，都能使用 pip 命令来安装下载。

- 如果不指定下载源，默认从官方的 PyPI 下载，也可以通过指定下载源来让 pip 从指定的 pypi 找包然后安装，比如有些公司可能有自己的源，如果想下载公司内部的工具包，就得在使用 pip 命令时加上源参数。
- PyPI，全称是：Python Package Index，它是Python官方的第三方库的仓库，所有人都可以下载第三方库或上传自己开发的库到 PyPI。

- Python 内置了 pip，Python 使用 pip 来安装包。pip 运行在 Python 上。

### npm

- npm 全称是 Node Package(包) Manager(管理器)，连起来就是 Node.js 的包管理器，提供了对 Node.js 包的查找、下载、安装、卸载的功能。只要有人把某个代码模块打包放在一个叫做「npm Registry」的地方，其他人就可以从这个「npm Registry」中下载安装包，只要包存在于「npm registry」，都能使用 npm 命令来安装下载。如果不指定下载源，默认从官方的 「npm Registry」下载，也可以通过指定下载源来让 npm 从指定的 Registry 找包然后安装，比如有些公司可能有自己的源，如果想下载公司内部的工具包，就得在使用 npm 命令时加上源参数。

- Node.js 内置了 npm，Node.js 使用 npm 来安装 module 包，npm 是用 Javascript 写的，运行在 Node.js 上。