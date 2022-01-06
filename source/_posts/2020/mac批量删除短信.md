---
title: mac批量删除短信
date: 2020-03-19 09:49:20
tags: [mac, scripts, 短信, sms]
---

每天都会收到大量的垃圾短信，或者各种验证码，苹果推出的同步功能确实很好，可是删除短信却不能同步。为了解决这个问题，网上找到了一个办法。再次在记录一下。

<!-- more -->

## 使用工具

这里用到的是苹果的脚本功能。找到你 Mac 上的`脚本编辑器`
脚本来源于网上，但已经不知道作者是谁了...，我做了简单修改，因为原来的代码支持的是英文系统

1. 打开脚本编辑器
2. 新建脚本`command + n`
3. 把下面的脚本粘贴进去

```
tell application "Messages" to activate

tell application "System Events"
tell process "Messages"
delay 1



tell window 1 -- 再告诉 第一个窗口
	repeat 500 times
		key code 51 using {command down}
		delay 0.3
		click button "删除" of sheet 1
		delay 0.2
	end repeat
end tell




end tell
end tell
```

4. 保存，这里我建议保存到 iCloud
5. 运行`command + r`
6. 等待运行完成
