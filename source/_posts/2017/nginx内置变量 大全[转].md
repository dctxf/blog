---
title: nginx内置变量 大全[转]
date: Sun Jun 04 2017 14:27:51 GMT+0800 (CST)
updated: Sun Jun 04 2017 14:27:51 GMT+0800 (CST)
comments: 1
categories: code
tags: []
permalink: /nginx-variable
---

最近在配置 nginx 代理 Google analytics 的时候用到了 nginx 变量，于是在网上搜了一下，发现了这篇文章，就转了过来，不过当中有些不是很清楚，等到用到的时候再补充，这里先做备份了。原文地址在文章最下面。

<!-- more -->

## nginx 内置变量

内置变量存放在`ngx_http_core_module`模块中，变量的命名方式和 apache 服务器变量是一致的。总而言之，这些变量代表着客户端请求头的内容，例如`$http_user_agent`, `$http_cookie`, 等等。下面是 nginx 支持的所有内置变量：

### \$arg_name

请求中的的参数名，即“`?`”后面的`arg_name=arg_value`形式的`arg_name`

### \$args

请求中的参数值

### \$binary_remote_addr

客户端地址的二进制形式, 固定长度为 4 个字节

### \$body_bytes_sent

传输给客户端的字节数，响应头不计算在内；这个变量和`Apache`的`mod_log_config`模块中的“`%B`”参数保持兼容

### \$bytes_sent

传输给客户端的字节数 (1.3.8, 1.2.5)

### \$connection

TCP 连接的序列号 (1.3.8, 1.2.5)

### \$connection_requests

TCP 连接当前的请求数量 (1.3.8, 1.2.5)

### \$content_length

`Content-Length` 请求头字段

### \$content_type

`Content-Type` 请求头字段

### \$cookie_name

cookie 名称

### \$document_root

当前请求的文档根目录或别名

### \$document_uri

同 \$uri

### \$host

优先级如下：HTTP 请求行的主机名 > ”HOST”请求头字段 > 符合请求的服务器名

### \$hostname

主机名

### \$http_name

匹配任意请求头字段； 变量名中的后半部分“name”可以替换成任意请求头字段，如在配置文件中需要获取 http 请求头：“`Accept-Language`”，那么将“－”替换为下划线，大写字母替换为小写，形如：`$http_accept_language`即可。

### \$https

如果开启了 SSL 安全模式，值为“on”，否则为空字符串。

### \$is_args

如果请求中有参数，值为“?”，否则为空字符串。

### \$limit_rate

用于设置响应的速度限制，详见 limit_rate。

### \$msec

当前的 Unix 时间戳 (1.3.9, 1.2.6)

### \$nginx_version

nginx 版本

### \$pid

工作进程的 PID

### \$pipe

如果请求来自管道通信，值为“p”，否则为“.” (1.3.12, 1.2.7)

### \$proxy_protocol_addr

获取代理访问服务器的客户端地址，如果是直接访问，该值为空字符串。(1.5.12)

### \$query_string

同 \$args

### \$realpath_root

当前请求的文档根目录或别名的真实路径，会将所有符号连接转换为真实路径。

### \$remote_addr

客户端地址

### \$remote_port

客户端端口

### \$remote_user

用于 HTTP 基础认证服务的用户名

### \$request

代表客户端的请求地址

### \$request_body

客户端的请求主体

此变量可在`location`中使用，将请求主体通过`proxy_pass`, `fastcgi_pass`, `uwsgi_pass`, 和 `scgi_pass`传递给下一级的代理服务器。

### \$request_body_file

将客户端请求主体保存在临时文件中。文件处理结束后，此文件需删除。如果需要之一开启此功能，需要设置 client_body_in_file_only。如果将次文件传递给后端的代理服务器，需要禁用`request body`，即设置

```
proxy_pass_request_body off;
fastcgi_pass_request_body off;
uwsgi_pass_request_body off;
//or
scgi_pass_request_body off;
```

### \$request_completion

如果请求成功，值为”OK”，如果请求未完成或者请求不是一个范围请求的最后一部分，则为空。

### \$request_filename

当前连接请求的文件路径，由 root 或 alias 指令与 URI 请求生成。

### \$request_length

请求的长度 (包括请求的地址, http 请求头和请求主体) (1.3.12, 1.2.7)

### \$request_method

HTTP 请求方法，通常为“GET”或“POST”

### \$request_time

处理客户端请求使用的时间 (1.3.9, 1.2.6); 从读取客户端的第一个字节开始计时。

### \$request_uri

这个变量等于包含一些客户端请求参数的原始 URI，它无法修改，请查看\$uri 更改或重写 URI，不包含主机名，例如：”/cnphp/test.php?arg=freemouse”。

### \$scheme

请求使用的 Web 协议, “http” 或 “https”

### \$sent_http_name

可以设置任意 http 响应头字段； 变量名中的后半部分“name”可以替换成任意响应头字段，如需要设置响应头 Content-length，那么将“－”替换为下划线，大写字母替换为小写，形如：`$sent_http_content_length 4096`即可。

### \$server_addr

服务器端地址，需要注意的是：为了避免访问 linux 系统内核，应将 ip 地址提前设置在配置文件中。

### \$server_name

服务器名

### \$server_port

服务器端口

### \$server_protocol

服务器的 HTTP 版本, 通常为 “HTTP/1.0” 或 “HTTP/1.1”

### \$status

HTTP 响应代码 (1.3.2, 1.2.2)

### $tcpinfo_rtt, $tcpinfo_rttvar, $tcpinfo_snd_cwnd, $tcpinfo_rcv_space

客户端 TCP 连接的具体信息

### \$time_iso8601

服务器时间的 ISO 8610 格式 (1.3.12, 1.2.7)

### \$time_local

服务器时间（LOG Format 格式） (1.3.12, 1.2.7)

### \$uri

请求中的当前 URI(不带请求参数，参数位于`$args`)，可以不同于浏览器传递的`$request_uri`的值，它可以通过内部重定向，或者使用 index 指令进行修改，\$uri 不包含主机名，如”`/foo/bar.html`”。

> 原文 http://www.cnphp.info/nginx-embedded-variables-lasted-version.html?utm_source=tuicool&utm_medium=referral
