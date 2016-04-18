---
layout: post
title:  "用 iTunes 从 App Store 下载某个 app 的任意版本"
date:   2016-04-17 20:00:00 +0800
categories: ios 
lang:   zh-CN
---

这个方法其实是 2015 年 12 月由[威锋的 lcz970](http://bbs.feng.com/read-htm-tid-10125110.html) 发现的，[Reddit](https://www.reddit.com/r/jailbreak/comments/3y6ax5/tutorial_how_to_legally_download_any_previous/) 只是转帖，我想当然地以为是 Reddit 用户先发现的了。到现在为止，已经过去四个多月了，没想到这个方法还能使用，<del>完全不是阿婆的风格嘛</del>。自己按照教程成功下到了几个国产<del>流氓</del>软件的历史版本，顺便做一下笔记。

基本原理是拦截 iTunes 向 Apple 服务器的请求，修改一个参数之后重发，所以需要一个抓包软件。网上流传的教程主要有使用 Charles 和 Fiddler 两种，本文记载后者。

1. 打开 Fidder，进入`Tools > Fidder Options > HTTP`，勾选`Capture HTTPS CONNECTs`和`Decrypt HTTPS traffic`。在弹出的对话框中选择`Yes`此时 UAC 会弹出提示，授权之后一路`确定`即可安装 Fiddler 的根证书。如果没有可以通过右侧的`Actions > Trust Root Certificate`安装。

2. 打开 iTunes，下载要下载的应用。在 Fidder 左侧的列表中看到`/WebObjects/MZBuy.woa`开头的项目之后，选中它，此时就可以停止并删除下载了。单击右侧浅黄色的`Response body is encoded. Click to decode.`解密响应。单击`Raw`以文本格式查看，把下面的窗格中的内容复制出来备用。

3. 在刚刚复制出来的响应中，找到`<key>softwareVersionExternalIdentifiers</key>`，它的下面有一长串全是数字的 array，如：

   ~~~xml
   <key>softwareVersionExternalIdentifier</key><integer>816868755</integer>
   <key>softwareVersionExternalIdentifiers</key>
   <array>
     <integer>815518554</integer>
     <integer>815692578</integer>
     <integer>815708364</integer>
     <integer>815712582</integer>
     <integer>816323800</integer>
     <integer>816462171</integer>
     <integer>816681537</integer>
     <integer>816727408</integer>
     <integer>816868755</integer>
     <integer>817018326</integer>
   </array>
   ~~~
   
   这些数字就是历史版本的 ID，越往后版本越新，最后一个一般是最新版本。可以参考 App Store 的更新记录找到要下载的版本的 ID，但是需要注意的是一个版本号可能不对应一个 ID。比如要下载 5 个版本号之前的版本，可能要往前数 10 个 ID。

4. 按`F11`（或者`Rules > Automatic Breakpoints > Before Requests`）暂停网络连接。在 iTunes 中下载要下载的应用。反复点击工具栏的 `Go` 按钮，直到看到列表中出现`/WebObjects/MZBuy.woa`开头的项目。

5. 在右侧上方的窗格中选择`TextView`，找到`<key>appExtVrsId</key>`，把下面对应`string`中的内容改为某个历史版本号的 ID，比如把

   ```xml
   <key>appExtVrsId</key>
   <string>817018326</string>
   ```
    
   改为
   
   ```xml
   <key>appExtVrsId</key>
   <string>816868755</string>
   ```
   
   点击下方绿色的`Run to Completion`恢复连接。

6. 按`Shift + F11`（或者`Rules > Automatic Breakpoints > Disabled`），点击工具栏的 `Go` 按钮，恢复正常连接，iTunes 应该已经开始下载了。

7. 在`%USERPROFILE%\Music\iTunes\iTunes Media\Mobile Applications`找到刚刚下载的应用，看看版本号是不是想要下载的那个。如果不是，回到第 3 步换一个 ID 重试。

8. 如果你觉得不安全的话，删掉 Fiddler 的证书：`开始 > 管理证书 > 受信任的根证书颁发机构 > 证书`，找到 `DO_NOT_TRUST_FiddlerRoot`，右键`删除`。

-----
后来我又读了一遍原教程，发现原教程提供了一个找 ID 更方便的方法，不用每次都等下载完成了。

在左侧的列表中找到`/WebObjects/MZBuy.woa`开头的项目，按`E`（或者`右键 > Replay > Reissue and edit`），在右侧上方的窗格中把`<key>appExtVrsId</key>`下面对应的`string`替换成某个 ID 之后，点击下方绿色的`Run to Completion`。响应出现后，点击浅黄色的`Response body is encoded. Click to decode.`解密，选择`RAW`或者`XML`，找到`<key>bundleShortVersionString</key>`（可以用下面的查找框），对应的`string`值就是版本号。比如：

~~~xml
    <key>bundleShortVersionString</key><string>6.4.0</string>
~~~

版本号就是 6.4.0。