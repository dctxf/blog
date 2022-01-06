---
title: Supervisor 网页管理器不可用解决办法
date: 2020-03-05 17:02:22
tags: [python, Supervisor]
---

在配置 python 环境的时候 Supervisor 的网页管理器总是不能访问，查找了一些资料后原来是自己的 NGINX 配置有问题。

<!-- more -->

## Supervisor 配置

```
[inet_http_server]
port = 9001
username = user
password = 123456
```

## nginx 配置

```
location ^~ /supervisor {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    # hack the host https://github.com/Supervisor/supervisor/issues/251
    proxy_set_header Host $http_host/supervisor/index.html;
    proxy_redirect off;
    rewrite ^/supervisor(.*) /$1 break;
    proxy_pass http://127.0.0.1:9001/;
}
```
