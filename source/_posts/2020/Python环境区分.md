---
title: Python环境区分
date: 2020-03-05 16:15:26
tags: [python, env, pipenv, gunicorn, Supervisor]
---

开发中常遇到各种开发环境，刚接触 python 不是很熟悉 python 的做法，折腾了很久，终于找到了一种办法，做个简单记录。

<!-- more -->

## 前情提要

我的项目主要使用了以下技术

- python3.7，python3.8
- pipenv
- gunicorn
- Supervisor

## 环境配置文件

使用`.ini`文件作为配置文件，在根目录下创建以下几个配置文件

```
# 默认
config.ini
# dev
config.dev.ini
# test
config.test.ini
```

配置文件内容如下

```
[APP]
DEBUG = True

[MYSQL]
USER = root
PASSWD = 123456
```

## 配置文件工具

### 原理

获取命令传入的参数，并取到自己想要的参数值作为环境变量值

### 具体代码

```python
import configparser
import os
import sys

# 获取参数
env = ''
for item in sys.argv:
    if item.find('-env') > -1:
        env = item.split('=')[1]
        print(item.split('='))
        break

# 寻找配置文件
cfg = configparser.RawConfigParser()
ini = 'config.ini' if not env else f'config.{env}.ini'
config_path = os.path.join(os.getcwd(), ini)
# 读取配置
cfg.read(config_path, 'utf-8')

```

### 使用配置文件

引入后即可使用`cfg['APP']['DEBUG']`的方式使用

下面用 logger 例子

```python
import logging

"""
引入工具
"""
from myapp.utils.cfg import cfg

LOG_MAP = {
    'INFO': logging.INFO,
    'WARN': logging.WARN,
    'ERROR': logging.ERROR,
    'DEBUG': logging.DEBUG
}
# Enable logging
logging.basicConfig(format=cfg['LOG']['FORMAT'], level=LOG_MAP[cfg['LOG']['LEVEL']])
logger = logging.getLogger(__name__)

```

## 启动命令

```bash
pipenv run gunicorn app:app --workers=2 --bind=0.0.0.0:5000 -env=dev
```
