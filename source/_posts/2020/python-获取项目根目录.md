---
title: python 获取项目根目录
date: 2020-03-09 10:41:21
tags: [python]
---

最近在学习 python 的过程中，配置环境变量的时候，我想要获取到 python 项目的根目录，因为我的配置文件是放在根目录的，但是我的工具类并不在根目录。网上找了一些方法，终于找到一个类似的。

<!-- more -->

## 原理

找到当前目录是很简单的，然后通过项目名截取就行了

## 代码

```
from os import path

"""
获取项目根路径
"""


def get_project_path(app_name):
    cur_path = path.abspath(path.dirname(__file__))
    return cur_path[:cur_path.find(app_name)]
```
