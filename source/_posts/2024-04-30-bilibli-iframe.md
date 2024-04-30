---
title: bilibili视频使用iframe嵌入Markdown的最佳方案
date: 2024-04-30 22:01:00
tags:
    - 建站
    - 技巧
categories:
    - 建站
description:
cover: //www.jiejisuan.com/images/upload/image/20180913/20180913110552_13874.jpg
---

# bilibili视频使用iframe嵌入Markdown的最佳方案

## 直奔主题，最佳方案代码如下：

```md
<div style="position: relative; width: 100%; height: 0; padding-top: calc(100% * 9 / 16);">
<iframe style="position: absolute; left: 0; top: 0; width: 100%; height: 100%;" src="//player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>
</div>
```

<div style="position: relative; width: 100%; height: 0; padding-top: calc(100% * 9 / 16);">
<iframe style="position: absolute; left: 0; top: 0; width: 100%; height: 100%;" src="//player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>
</div>

## 原始方法代码及效果

```md
<iframe src="//player.bilibili.com/player.html?aid=561005338&bvid=BV19e4y1q7JJ&cid=846391446&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
```

<iframe src="//player.bilibili.com/player.html?aid=561005338&bvid=BV19e4y1q7JJ&cid=846391446&p=1&poster=1&autoplay=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## ~~过渡过程中使用的方法~~

这个方法虽然一开始看着好一点，但是只有使用haxo的博客可以用。所以通用性比较差

```md
{% iframe //player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0 %}
```

{% iframe //player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0 %}

## 参考

- [站外(外链)播放器使用文档](//player.bilibili.com)
- [网站嵌入bilibili视频的一些总结](//www.bilibili.com/read/cv6775208/)
- 