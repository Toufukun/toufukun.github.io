---
layout: post
title:  "强制浏览器使用思源黑体的 Stylish 样式"
date:   2016-04-27 21:04:00 +0800
categories: font web
lang:   zh-CN
---
特点：

- Stylish 样式，适用于 Firefox 与 Chrome
- 强制微软雅黑、冬青黑体简体中文→思源黑体（简体），微软正黑→思源黑体（繁体）
- 对于那些使用默认字体（`sans-serif`）的网站，需配合浏览器字体设置使用
- 适用字体：Language-specific OTF (Style-linked, Regular & Bold)、Region-specific OTF（其他字重）
- Firefox 请关闭 DirectWrite、使用 MacType
- ExtraLight（简体）、Light、Regular、Bold、Heavy（简体）字重对应
- 请根据个人口味酌情修改，我不对任何 bug 负责

```css
@namespace url(http://www.w3.org/1999/xhtml);

@font-face {
  font-family: "Hiragino Sans GB";
  src: local("Source Han Sans SC");
  font-weight: 400;
}

@font-face {
  font-family: "Hiragino Sans GB";
  src: local("Source Han Sans SC Bold");
  font-weight: 700;
}

@font-face {
  font-family: "Microsoft YaHei";
  src: local("Source Han Sans SC");
  font-weight: 400;
}

@font-face {
  font-family: "Microsoft YaHei";
  src: local("Source Han Sans SC Bold");
  font-weight: 700;
}

@font-face {
  font-family: "Microsoft YaHei";
  src: local("Source Han Sans CN ExtraLight");
  font-weight: 200;
}

@font-face {
  font-family: "Microsoft YaHei";
  src: local("Source Han Sans CN Light");
  font-weight: 300;
}

/* @font-face {
  font-family: "Source Han Sans SC";
  src: local("Source Han Sans CN Light");
  font-weight: 300;
} */

@font-face {
  font-family: "Microsoft YaHei Light";
  src: local("Source Han Sans CN Light");
}

@font-face {
  font-family: "Microsoft YaHei UI Light";
  src: local("Source Han Sans CN Light");
}

@font-face {
  font-family: "Microsoft YaHei";
  src: local("Source Han Sans CN Normal");
  font-weight: 350;
}

@font-face {
  font-family: "Microsoft YaHei";
  src: local("Source Han Sans CN Heavy");
  font-weight: 900;
}

@font-face {
  font-family: "Microsoft JhengHei";
  src: local("Source Han Sans TC");
  font-weight: 400;
}

@font-face {
  font-family: "Microsoft JhengHei";
  src: local("Source Han Sans TC Bold");
  font-weight: 700;
}

@font-face {
  font-family: "Microsoft JhengHei";
  src: local("Source Han Sans TW Light");
  font-weight: 300;
}

@font-face {
  font-family: "Microsoft JhengHei Light";
  src: local("Source Han Sans TW Light");
}
```