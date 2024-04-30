---
title: github page 服务优化
date: 2024-04-30 11:32:07
tags:
    - Hexo
    - Butterfly
    - 建站
categories:
    - 建站
description:
cover: //camo.githubusercontent.com/d088d0ad0eb663726111092051df4bc2d3f29b7a12e0682c2900c47fb7d47d75/68747470733a2f2f63646e2e6a7364656c6976722e6e65742f67682f6a65727279633132372f43444e406d322f696d672f7468656d652d627574746572666c792d726561646d652e706e67
---

# github page 服务优化

启动本地服务，哇塞可好看了，还挺合适，一推送到github怎么这么慢。

博主根据[butterfly文档][butterfly]配置了一部分。

以下是关于优化的配置合集希望对大家有所帮助

## 使用Pjax进行页面跳转

> [butterfly文档中关于pjax的配置][butterfly-pjax]

当用户点击链接，通过ajax更新页面需要变化的部分，然后使用HTML5的pushState修改浏览器的URL地址。

这样可以不用重复加载相同的资源（css/js）， 从而提升网页的加载速度。

```yml
# Pjax
# It may contain bugs and unstable, give feedback when you find the bugs.
# https://github.com/MoOx/pjax
pjax:
  enable: true
  # exclude:
  #   - /music/
  #   - /no-pjax/
```

> 当前对pjax的支持仍有部分问题，你可以把有问题的网页链接加入到`exclude`里，这个网页会被pjax排除在外。
> 当使用pjax后可能存在以下问题：
> - 使用谷歌广告可能会报错（例如自动广告，[广告相关配置][butterfly-ad]）
> - 对于一些第三方插件，有些并不支持pjax。（那些第三方插件？）
> - 一些自己DIY的js可能会无效，跳转页面时需要重新调用，请参考[Pjax文档][Pjax]
> - 一些个别页面加载的js/css，将会改为所有页面都加载


## 使用Instantpage预加载链接

> [butterfly文档中关于Instantpage的配置][butterfly-instantpage]

当鼠标悬停到链接上超过 65 毫秒时，Instantpage会对该链接进行预加载，可以提升访问速度。

```yml
# https://instant.page/
# prefetch (预加载)
instantpage: true
```

## 图片懒加载

> 没找到[butterfly文档][butterfly]中关于这一部分的配置描述

```yml
# Lazyload (圖片懶加載)
# https://github.com/verlok/vanilla-lazyload
lazyload:
  enable: true
  field: site # site/post
  placeholder: # 图片加载过程中的占位图片链接
  blur: true
```

## 图片压缩

> [butterfly文档中推荐的图片压缩工具][butterfly-zipimg]

Butterfly主题需要使用到很多图片。如果图片太大，会严重拖慢网站的加载速度。图片压缩能够有效的缓解这个问题。

### 使用powertoy内置工具压缩图片

博主使用[Microsoft PowerToys][PowerToys]的Image Resizer工具进行图片压缩。请参考Image Resizer实用工具的详细使用[教程][PowerToys-imgrz]

### 使用imgbot自动压缩仓库中的图片

imgbot是一款Github插件。

安装后，你上传图片到Github去，imgbot会自动压缩图片并推送PR，我们只需要合并PR

你可以配置imgbot的侦测方法、压缩方法（有损/无损），具体可以查看插件的文档

![](https://jsd.012700.xyz/gh/jerryc127/CDN/img/butterfly-enhance-imgbot.png)

[butterfly]: <//butterfly.js.org/> (butterfly主题配置文档)
[butterfly-pjax]: <//butterfly.js.org/posts/ceeb73f/#Pjax> (butterfly主题配置文档中关于pjax部分的配置)
[butterfly-instantpage]: <//butterfly.js.org/posts/ceeb73f/#Instantpage> (butterfly主题配置文档中关于instantpage部分的配置)
[butterfly-zipimg]: <//butterfly.js.org/posts/4073eda/#%E5%9C%96%E7%89%87%E5%A3%93%E7%B8%AE> (butterfly主题配置文档中关于图片压缩的介绍)
[butterfly-ad]: <//https://butterfly.js.org/posts/ceeb73f/#%E5%BB%A3%E5%91%8A> (butterfly主题配置文档中关于广告的配置)
[Pjax]: <//github.com/MoOx/pjax> (Pjax文档)
[PowerToys]: <//github.com/microsoft/PowerToys> (PowerToys)
[PowerToys-imgrz]: <//learn.microsoft.com/zh-cn/windows/powertoys/image-resizer> (Image Resizer实用工具)
