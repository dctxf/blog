---
title: "Big Sur PD虚拟机安装 [BIErrorDomain错误3]"
date: 2020-06-29 10:07:22
tags:
---

为了尝鲜用虚拟机装个系统，中间遇到这个错误

<!-- more -->

## 错误问题

### [OS-MacOS] Parallels 15（Mac）：将 macOS Catalina Beta 更新为 Big Sur Beta [BIErrorDomain 错误 3]

#### 解决方案（VMware Fusion）

设置参数

```
smbios.reflectHost = "TRUE"
```

#### 解决方案（Parallels Desktop）

##### 1. 在真实的主机上的终端输入以下命令

```
ioreg -l | grep board-id
```

把得到的结果记录下来，例如我的是`Mac-1E7E29AD0135F9BC`

##### 2. 再在终端上输入

```
sysctl hw.model
```

并记录值,例如我的值是`MacBookPro15,3`

#### 3. 最后

用上面的两个值拼成下面的样子

```
devices.mac_hw_model="MacBookPro15,3"
devices.smbios.board_id="Mac-1E7E29AD0135F9BC"
```

把这段复制到 pd 虚拟机的`硬件-启动顺序-高级设置-引导标识`粘贴进去，再次启动即可

## 参考链接

[[OS - 🍎 macOS] Parallels 15 (Mac): Updating macOS Catalina Beta to Big Sur Beta [BIErrorDomain error 3]](https://dev-dream-world.tistory.com/139)
