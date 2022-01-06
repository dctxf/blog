---
title: CentOS6 安装docker
date: Thu May 25 2017 14:19:26 GMT+0800 (CST)
updated: Thu May 25 2017 14:19:26 GMT+0800 (CST)
comments: 1
categories: code
tags: []
permalink: /centos6-install-docker
---

Docker 是个划时代的开源项目，它彻底释放了计算虚拟化的威力，极大提高了应用的运行效率，降低了云计算资源供应的成本！ 使用 Docker，可以让应用的部署、测试和分发都变得前所未有的高效和轻松！

无论是应用开发者、运维人员、还是其他信息技术从业人员，都有必要认识和掌握 Docker，以在有限的时间内做更多有意义的事。

<!-- more -->

## 首先查看系统版本

```bash
lsb_release -a

# LSB Version:	:base-4.0-amd64:base-4.0-noarch:core-4.0-amd64:core-4.0-noarch
# Distributor ID:	CentOS
# Description:	CentOS release 6.8 (Final)
# Release:	6.8
# Codename:	Final
```

## 添加 yum 源

```
sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7/
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF
```

要在 CentOS-6 上安装 docker，请利用以下指令安装 docker-io 组件：

```bash
sudo yum install docker-io
```

安装 docker 后，你必须引导该服务才能应用它。

```bash
sudo service docker start
```

若要开机时引导 docker 服务：

```bash
sudo chkconfig docker on
```

## 拓展阅读

- [DOCKER 学习笔记](https://notes.dctxf.com/code/docker/)
- [Docker — 从入门到实践](https://yeasy.gitbooks.io/docker_practice/content/)
