---
title: bilibili视频使用iframe嵌入Markdown的最佳方案
date: 2024-04-30 22:01:00
updated: 2024-04-30 22:01:00
tags:
    - 建站
    - 技巧
categories:
    - [建站]
description:
cover: //www.jiejisuan.com/images/upload/image/20180913/20180913110552_13874.jpg
---

# bilibili视频使用iframe嵌入Markdown的最佳方案

## 直奔主题，最佳方案代码及效果如下：

```md
<iframe style="width: 100%; aspect-ratio: 16/9;" src="//player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>
```

使用style属性使视频窗口的宽度页面的宽度保持一致，固定视频的宽高比。从而保证视频能够在页面中有一个比较好的响应式布局。

<iframe style="width: 100%; aspect-ratio: 16/9;" src="//player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>

## 原始方法代码及效果

```md
<iframe src="//player.bilibili.com/player.html?aid=561005338&bvid=BV19e4y1q7JJ&cid=846391446&p=1&poster=1&autoplay=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
<!-- 在原代码的基础上加入了 &poster=1&autoplay=0 两个参数，用于展示封面并关闭视频自动播放 -->
```

<iframe src="//player.bilibili.com/player.html?aid=561005338&bvid=BV19e4y1q7JJ&cid=846391446&p=1&poster=1&autoplay=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

## ~~过渡过程中使用的方法~~

下面很多都是参考自大佬的博客：[CSS实现长宽比的几种方案](https://www.cnblogs.com/thinkingthigh/p/15723303.html)

### 传统方法

```md
<div style="position: relative; width: 100%; height: 0; padding-top: calc(100% * 9 / 16);">
<iframe style="position: absolute; left: 0; top: 0; width: 100%; height: 100%;" src="//player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>
</div>
```

<div style="position: relative; width: 100%; height: 0; padding-top: calc(100% * 9 / 16);">
<iframe style="position: absolute; left: 0; top: 0; width: 100%; height: 100%;" src="https://player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>
</div>

### 基于hexo

这个方法虽然一开始看着好一点，但是只有使用haxo的博客可以用。所以通用性比较差

```md
{% iframe //player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0 %}
```

{% iframe //player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0 %}

### 使用css

由于上面的写法太过冗长（其实复制粘贴就可以了），所以就想着使用css来实现这个效果。但是我转念一想，我们可能都是想临时用一下。而且我希望我写的文章可以长久的保存在markdown文件里（所以文章中引用图片到底咋弄我还在想），所以我更推荐上面的方法，下面的方法供大家学习好了。

```css
[bl-w-h] {
    /* --bl-w-h: attr(bl-w-h number, clac(100%*16/9)); */
    --bl-w-h: 16/9
}
[bl-w-h="1/1"] {
    --bl-w-h: 1/1
}
[bl-w-h="4/3"] {
    --bl-w-h: 4/3
}
[bl-w-h], [style*="--bl-w-h"] {
    position: relative;
    width: 100%;
    height: 0;
    padding-top: calc(100%/(var(--bl-w-h)));
    > * {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
    }
}
```

写这段css学了一堆知识，又是在知识的海洋里遨游的一天。

这个css可以实现使用html属性或者css变量调整窗口的比例。（突然想起了css现在好像支持一个参数，但是我试都没试一下，我先哭一会）

我去试了一下，真的可以，我将其放到本文的开头部分了。也可参考这篇文章[css 新属性：aspect-ratio](https://zhuanlan.zhihu.com/p/348596969)。

```md
<div bl-w-h>
<!-- <div bl-w-h="4/3"> --><!-- 使用html属性调节窗口比例 -->
<!-- <div style="--bl-w-h:4/3;"> --><!-- 使用css变量调节窗口比例 -->
<iframe src="//player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>
</div>
```

<style>
[data-bl-w-h],
[style*="--bl-w-h"] {
    /* 这个没有作用 */
    --bl-w-h: attr(data-bl-w-h number, 16/9); 
    position: relative;
    width: 100%;
    height: 0;
    padding-top: calc(100%/(var(--bl-w-h)));

    >* {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
    }
}
[data-bl-w-h] {--bl-w-h: 16/9;}
[data-bl-w-h="4/3"] {--bl-w-h: 4/3;}
[data-bl-w-h="1/1"] {--bl-w-h: 1/1;}

/* [bl-w-h], [style*="--bl-w-h"] {
    position: relative;
    box-sizing: border-box;
    > * {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        box-sizing: border-box;
        width: 100%;
        height: 100%
    }
    &::before {
        position: relative;
        display: block; 
        content: "";
        padding-top: calc(100%/(var(--bl-w-h)));
        box-sizing: border-box;
    }
} */
</style>
<div data-bl-w-h>
<iframe src="//player.bilibili.com/player.html?bvid=BV19e4y1q7JJ&poster=1&autoplay=0" frameborder="no" scrolling="no"></iframe>
</div>

## 大家一起学CSS

尽管到头来白忙一场，还是学到了一些东西，而且我们找到了更好的方式。
- css变量：`--var-name`
- css函数：`calc()`、`var()`、`attr()`
- `:is()`伪类和`::before`伪元素
- css嵌套


## 参考

- [站外(外链)播放器使用文档](//player.bilibili.com)
- [网站嵌入bilibili视频的一些总结](//www.bilibili.com/read/cv6775208/)
- [CSS实现长宽比的几种方案](https://www.cnblogs.com/thinkingthigh/p/15723303.html)
- [css 新属性：aspect-ratio](https://zhuanlan.zhihu.com/p/348596969)
