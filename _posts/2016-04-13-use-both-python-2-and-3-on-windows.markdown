---
layout: post
title:  "如何在 Windows 中同时使用 Python 2.x 和 3.x？"
date:   2016-04-13 21:10:00 +0800
categories: python windows
lang:   zh-CN
---
1. 安装 Python 2.x 到默认路径，比如`C:\Python27`。
2. 安装 Python 3.x 到默认路径，比如`C:\Program Files (x86)\Python35-32`。
3. 查看 Python 3 的 pip 版本，`python -m pip --version`。如果还是 7.x.x 的话，使用 `python -m pip install --upgrade pip` 升级到 8.x.x。因为旧版本有路径中含有空格就无法运行的 bug。
4. 把 Python 3 的 `python.exe` 改名为 `python3.exe`；`Scripts\pip.exe` 删掉，只留下 `pip3.exe`。
5. 把 Python 2 和 3 的目录都加入环境变量，大功告成。

效果：
{% highlight text %}
C:\>python --version
Python 2.7.10

C:\>python3 --version
Python 3.5.1

C:\>pip --version
pip 8.1.1 from C:\Python27\lib\site-packages (python 2.7)

C:\>pip3 --version
pip 8.1.1 from c:\program files (x86)\python35-32\lib\site-packages (python 3.5)
{% endhighlight %}

<br>

PS: 我才知道原来 Windows 也可以用 shebang 指定执行脚本的解释器。

{% highlight python %}
#!C:/Program Files (x86)/Python35-32/python3.exe
import platform
print(platform.python_version())
{% endhighlight %}

运行结果：
{% highlight text %}
3.5.1
{% endhighlight %}
