---
layout: post
title:  "用 Fiddler 抓包的几个必要条件"
date:   2016-04-22 21:51:00 +0800
categories: fiddler http
lang:   zh-CN
---

Fiddler 不是一款全能的抓包软件，其实它是一个针对 HTTP 连接的 Web 调试工具，所以使用的时候有一定条件。

1. 只支持 HTTP 和 HTTPS，看不到其他协议的通信。
2. 软件支持 HTTP 代理，对于不能检测系统代理的软件，需要将它的代理设置为`127.0.0.1:8888`。原有的系统代理会被 Fiddler 自动用作前置代理。例如：
   - 某些 Java 软件不会自动应用代理，需要手工设置
   - 浏览器不能使用代理切换插件
   - 很多命令行工具默认不使用代理，需要加额外的开关和参数
3. 要解密 HTTPS 通信内容，需要在系统中信任 Fiddler 的根证书。
   - 一般而言，在 Fiddler 中让系统信任他的证书就行了。
   - 然而有些软件并不会读取系统的证书，例如 Python。Python 只信任`Python 安装目录/Lib/site-packages/certifi/cacert.pem`中的证书，所以要想在 Python 中使用 HTTPS，需要在这个文件中加入 Fiddler 的证书，否则 Python 将因为证书错误无法建立 HTTPS 连接。
   - Fiddler 生成到桌面的证书是二进制的，我们需要把它转换成 Base64 格式的。首先把证书导入系统，进入 Windows 的证书管理，在“受信任的根证书颁发机构”里找到`DO_NOT_TRUST_FiddlerRoot`，单击`复制到文件`，然后`不，不要导出私钥`，选择你`Base64 编码 X.509(.CER)`，保存证书即可。
   - 用文本编辑器打开上面保存的证书，把内容复制粘贴到上面所说的`cacert.pem`的末尾，Python 便可以正确使用 HTTPS 通信了。使用完后可将`cacert.pem`恢复原样。
   
PS：没错，这篇文章是充数的……