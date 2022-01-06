---
title: Python环境搭建
date: 2020-03-05 10:29:45
tags: [python, pipenv, Gunicorn, Supervisor]
---

最近在自学 python，当中遇到了不少问题，环境是最让人头疼的地方，不同的机器出现不同的效果，这是头大，在此记录下自己的环境配置

<!-- more -->

## Python 安装

### Mac 环境

安装 python ，由于 Mac 已经自带 python2，而我要用 python3，所以需要先安装 python3，这里使用`homebrew`安装

```bash
# 以下命令会直接安装python的最高版本
brew install python
# 安装其他版本
brew install python@3.7
```

### Linux 环境 debian

为了少出错，建议用源码安装

#### 安装

登陆 Python 官网下载源码地址是<https://www.python.org/downloads/source/>
找到你想要下载的源码

```bash
# 下载源码
wget https://www.python.org/ftp/python/3.7.7/Python-3.7.7.tgz
# 解压源码
tar -zvxf Python-3.7.7.tgz
# 进入到解压后的文件夹
cd Python-3.7.7.tgz
# 配置
./configure --prefix=/usr/local/python3
# 编译并安装
make && make install
# 添加软链接
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
# 查看python版本
python3 -V
pip -V
```

## 虚拟环境 pipenv

- 安装方式参考官方 <https://github.com/pypa/pipenv>

### pipenv 常用命令

```bash
# 进入到虚拟环境
pipenv shell
# 安装python包
pipenv install 包名
# 安装开发依赖
pipenv install 包名 --dev
#例如：
pipenv install flask
# 运行
pipenv run python3 app.py
```

## 部署

部署就都在 Linux 环境下

### Gunicorn

- 官网 <https://gunicorn.org/>

1. 安装 Gunicorn ：

```bash
pipenv install gunicorn
```

2. 使用 Gunicorn 运行一个 WSGI 程序：

```bash
pipenv run gunicorn --workers=4 --bind=0.0.0.0:8000 app:app
# --workers = 4 表示使用 4 worker 进程运行程序，建议 worker 数量为 ( CPU 核心数 × 2 ) + 1
# Gunicorn 默认只允许从本地 8000 端口访问，--bind=0.0.0.0:8000 表示允许使用 8000 端口从外部访问
# app:app 冒号前面的 app 表示 app.py 文件，冒号后面的 app 表示 flask 程序的名称
也可以把 --workers 简写为 -w 、--bind 简写为 -b ，如下：

# 没有 -b 或者 --bind 参数，默认监听 127.0.0.1:8000
pipenv run gunicorn -w 4 app:app

# 指定 -b 0.0.0.0:8000 监听 8000 端口的外部请求
pipenv run gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

#### 注意

这里是后台启动关闭需要杀进程，我认为很不友好。管理进程还需要 Supervisor

### Supervisor

Supervisor 是一个客户端/服务器系统，允许其用户监视和控制类似 UNIX 的操作系统上的多个进程。

- 官网 <http://www.supervisord.org/>

#### 安装

**建议使用 Python3**

```
pip install supervisor
```

#### 配置

找到你的配置文件目录，有的在`/etc/supervisord.conf`,而我的在`/etc/supervisord/supervisord.conf`，增加以下配置，++_如果有修改即可_++

```
# 引入外部配置
[include]
files = /etc/supervisord.d/*.conf
# 在supervisord.conf中设置通过web可以查看管理的进程，加入以下代码（默认即有，取消注释即可）
[inet_http_server]
port=9001
username=user
password=123
```

增加完配置用下面的命令重载

```
supervisorctl reload
```

这样你就能通过界面进行访问了，地址为`http://localhost:9001`

#### 常用命令（嫌命令麻烦别着急）

```
# 启动
supervisord -c /etc/supervisord.conf
# 停止
supervisorctl shutdown
# 更新新的配置到supervisord
supervisorctl update
# 重新启动配置中的所有程序
supervisorctl reload
# 启动某个应用
supervisorctl start 应用名
# 停止某个应用
pervisorctl stop 应用名
# 重启某个应用
pervisorctl restart 应用名
# 停止全部进程
pervisorctl stop all
```

#### 项目配置

项目配置看你上面的配置目录在哪，这里在`/etc/supervisord.d`,进入到目录，配置你的项目文件

新建项目文件 `vim myapp.conf`

```
[program:app]
; 下面命令中的 app:app 请修改为你实际部署时的项目名称
command=pipenv run gunicorn --workers=2 --bind=0.0.0.0:5000 app:app

; 下面的路径请修改为你创建的项目的根目录
directory=/root/pushme

autostart=true
autorestart=true
stopsignal=QUIT
stopasgroup=true
killasgroup=true

; 下面的用户请修改为创建该项目的用户
user=root

redirect_stderr=true

; log 文件的路径你可以重新自定义
stdout_logfile=/var/logs/log/supervisor.log

; 解决编码问题
[supervisord]
environment=LC_ALL='en_US.UTF-8',LANG='en_US.UTF-8'
```

## 参考资料

- [CentOS 下使用 Pipenv + Gunicorn + Supervisor 部署 Flask 程序](https://segmentfault.com/a/1190000018104547#item-7)
- [Supervisor 重新加载配置启动新的进程](https://blog.csdn.net/shudaqi2010/article/details/51153961)
- [supervisor 用法](https://www.jianshu.com/p/535c22ea6e28)
- [进程管理工具 Supervisor 的使用](http://blog.cheyo.net/1.html)
