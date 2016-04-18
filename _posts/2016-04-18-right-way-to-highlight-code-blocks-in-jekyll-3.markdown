---
layout: post
title:  "在 Jekyll 3 里让列表内的代码块也拥有代码高亮的正确方法"
date:   2016-04-18 12:23:00 +0800
categories: jekyll kramdown 
lang:   zh-CN
---

我在写上一篇文章的时候发现要想在列表中插入一段代码，还要有语法高亮，是一件非常困难的事……我爬了许多文，包括 Stack Exchange、Jekyll 官方文档、[Github Markdown 写作指南](https://help.github.com/articles/creating-and-highlighting-code-blocks/)和 [kramdown 官方文档](http://kramdown.gettalong.org/syntax.html#code-blocks)，无一成功。有人建议换一种 markdown 或者语法高亮引擎，可是自 Jekyll 3 开始，rouge 成为了 Jekyll 的默认的代码高亮器，Github Pages 自 2016 年 1 月起也只支持 kramdown + rouge 的组合了。

在我想要放弃的时候，发现了[这篇文章](http://mazhuang.org/2016/02/04/switch-to-kramdown-from-redcarpet/#section)。

原来，只需要让列表中的所有文字保持同样的缩进就行了。

~~~~~markdown
1. 这是列表中的文字，我们想要插入一段代码

   ```python
   def hello():
       print("Hello, world!")
   ```
   
   因为这个条目第一行实际的文字缩进了三个字符，所以后续的几行（包括代码）也要缩进**三个字符（三个空格）**。
   
2. 序号不会断掉！

   但是要注意，代码前后要有空行。
~~~~~

效果：

1. 这是列表中的文字，我们想要插入一段代码

   ```python
   def hello():
       print("Hello, world!")
   ```
   
   因为这个条目第一行实际的文字缩进了三个字符，所以后续的几行（包括代码）也要缩进**三个字符（三个空格）**。
   
2. 序号不会断掉！

   但是要注意，代码前后要有空行。
   
提示：

- 在 kramdown 中，<code>```</code>和`~~~`其实效果是相同的。
- 如果代码中有<code>```</code>或者`~~~`的话，可以用更长的波浪线，比如<code>`````</code>或者`~~~~~`。
- 行内代码不能用`~`，可以用`<code></code>`。
- **最好将 tab 提前转换为空格。**kramdown 和 rouge 不能自动处理 tab，在浏览器中会显示为 8 个空格，如：

  ```
  1234567890
  	这行字是用 tab 缩进的
      这行字是用的四个空格
  ```

**不行**的方法：

- 混合使用原版 Markdown（八个空格）和 rouge 风格的 <code>```</code>：

  `````markdown
  1. 这是列表中的文字
  
          ```python
          print("hello world")
          ```
          
  `````
  
- 缩进不一致（使用四个空格或者一个 tab）：
  
  `````markdown
  1. 这是列表中的文字
  
      ```python
      print("hello world")
      ```
      
  `````

- 使用原版 Markdown（八个空格）风格加 kramdown 的 IAL 注释：

  `````markdown
  1. 这是列表中的文字
  
          print("hello world")
          {:.language-python}
  `````
  
- 用 kramdown 的 IAL 高亮行内代码段：  
  <code>`print("hello world");`{:.language-python}</code> → `print("hello world")`{:.language-python}

<br/>
总结：  
**缩进很重要！**一定要按照 kramdown 的方式缩进。