---
title: butterfly自定义半透明背景
date: 2024-05-03 09:35:46
tags:
    - CSS
    - Butterfly
    - Hexo
    - 建站
categories:
    - 建站
description:
cover: //jsd.012700.xyz/gh/jerryc127/CDN/img/CODE-COLOR-COVER.png
---

# butterfly自定义半透明背景

## 直奔主题

将下面的样式写入文件`source/-/css/bl-sun.css`

```css
[data-theme='light']{
    --card-bg: rgba(255, 255, 255, .8);
}
[data-theme='dark']{
    --card-bg: rgba(13, 13, 13, .6);
}
```

在`_config.butterfly.yml`配置文件中注入该样式

```yml
inject:
  head:
    - <link rel="stylesheet" href="/-/css/bl-sun.css">
```

## 前言

看[大佬的博客](//blog.270916.xyz)是半透明的背景，真的很好看。想着给自己的也弄一个。

## 探索

我查了[buttfly关于颜色的配置](https://butterfly.js.org/posts/ceeb73f/#%E8%87%AA%E5%AE%9A%E7%BE%A9%E4%B8%BB%E9%A1%8C%E8%89%B2)可以配置以下参数：

```yml
theme_color:
  enable: true
  main: "#49B1F5"
  paginator: "#00c4b6"
  button_hover: "#FF7242"
  text_selection: "#00c4b6"
  link_color: "#99a9bf"
  meta_color: "#858585"
  hr_color: "#A4D8FA"
  code_foreground: "#F47466"
  code_background: "rgba(27, 31, 35, .05)"
  toc_color: "#00c4b6"
  blockquote_padding_color: "#49b1f5"
  blockquote_background_color: "#49b1f5"
  scrollbar_color: "#49b1f5"
```

不知道有没有对夜间模式进行适配。于是我按下了<kbd>F12</kbd>，我想看看它的`background`样式是怎么控制。

于是我发现它是通过下面的CSS使用变量`--card-bg`设置了背景的颜色。

```css
.cardHover, .error404 #error-wrap .error-content, .layout > div:first-child:not(.recent-posts), #recent-posts > .recent-post-item, #aside-content .card-widget, .layout > .recent-posts .pagination > *:not(.space) {
    // ...
    background: var(--card-bg);
    // ...
}
```

找到了对应的CSS变量`--card-bg`，接着我们可以找到`--card-bg`变量的定义。

```css
[data-theme='dark'] {
    // ...
    --card-bg: #121212;
    // ...
}
```

通过切换主题夜间模式，可以找到日间模式的`--card-bg`变量定义。

```css
:root {
    // ...
    --card-bg: #fff;
    // ...
}
```

我们可以找到`data-theme`属性的位置，是在`html`标签上的。

```html
<html lang="zh-CN" data-theme="light"></html>
```

## 配置

最终我们可以通过注入css的方式来使我们的背景颜色透明。

将下面的样式写入文件`source/-/css/bl-sun.css`

```css
[data-theme='light']{
    --card-bg: rgba(255, 255, 255, .8);
}
[data-theme='dark']{
    --card-bg: rgba(13, 13, 13, .6);
}
```

> 这里没有使用[`:root`伪类](//developer.mozilla.org/zh-CN/docs/Web/CSS/:root)设置日间模式的背景颜色。
> 这里设置的颜色是将原来**Hex格式**的颜色通过[RGB颜色查询对照表](//www.qianbo.com.cn/Tool/Rgba/)转化为**CSS格式**然后调节`alpha`值使其变得透明。

在`_config.butterfly.yml`配置文件中注入该样式

```yml
inject:
  head:
    - <link rel="stylesheet" href="/-/css/bl-sun.css">
```

## 进一步

通过搜索`--card-bg`，可以发现`themes/butterfly/source/css/_global/index.styl`中对其进行了初始化

```styl
:root
  --card-bg: $card-bg
```

进一步，通过搜索`$card-bg`可以在`themes/butterfly/source/css/var.styl`查到如下代码。

```styl
$white = #FFFFFF
$card-bg = $white
```

不懂`.styl`文件的语法，但是看起来是写死了的。

还是在`themes/butterfly/source/css/var.styl`文件中可以找到如下代码

```styl
$theme-color = $themeColorEnable && hexo-config('theme_color.main') ? convert(hexo-config('theme_color.main')) : $bright-blue
```

看来是根据配置文件中设置的`theme_color.main`配置变量`$theme-color`。如此对比，可以说`$card-bg`就是写死了的，看来我们的方法还是挺“正规”的。

## 最后

感觉butterfly这部分代码有优化的空间，想要尝试给大佬做一下贡献，吼吼。

具体来说，就下面这段代码看起来重复了好多。由于一些变量如`$card-bg`没有使用下面这种形式的写法，导致无法自定义。如果有方法可以自动生成下面这种类型的代码，那灵活性应该会有很大的提升。

```styl
$theme-color = $themeColorEnable && hexo-config('theme_color.main') ? convert(hexo-config('theme_color.main')) : $bright-blue
```

### 可行性分析

- styl语法是否支持

如果支持的话，首先获取所有以`theme_color`开头的属性列表。然后遍历这个列表
伪代码：

```styl
list = hexo-config-list()
for i in list:
    ${i} = $themeColorEnable && hexo-config(i) ? convert(hexo-config(i)) : ${i}
```


```styl
reset(args)
    for arg in args
        $theme-{i} = $themeColorEnable && hexo-config('theme_color.{i}') ? convert(hexo-config('theme_color.{i}')) : $theme-{i}
```

### 成果展示

在`themes\butterfly\scripts\events\stylus.js`中注册一个`styl`函数

```js
hexo.extend.filter.register('stylus:renderer', style => {
  style.define('$format_css', name => {return String(name).replace(/_/g, '-');})
})
```

> 这段代码就是注册了一个`$format_css`函数，它被用来将字符中的`_`替换为`-`
> 不用`String()`的话会报错，不知道是什么原因。主要参考了[`hexo-config()`](https://github.com/hexojs/hexo-renderer-stylus/blob/master/lib/renderer.js)的代码。
> 本来想的是照着它这种读取的方式自己读取的，后来自己一调用`hexo-config()`就可以得到相应的键值对呢。白忙活半天

在`themes\butterfly\source\css\index.styl`中加入如下测试代码

```styl
//test
:root
  for prop in hexo-config('theme_color')
    --theme-{$format_css(prop[0])} convert(prop[1]) unless prop[0]=='enable'
```
> 哦对，`hexo-config('theme_color')`要是`yml`里有`theme_color.light.card_bg`这种三级的配置的话好像不知道会出现什么问题哎。没有尝试。

我的配置文件

```yml
theme_color:
  enable: true
  main: "#49B1F5"
  paginator: "#00c4b6"
  button_hover: "#FF7242"
  text_selection: "#00c4b6"
  link_color: "#99a9bf"
  meta_color: "#858585"
  hr_color: "#A4D8FA"
  code_foreground: "#F47466"
  code_background: "rgba(27, 31, 35, .05)"
  toc_color: "#00c4b6"
  blockquote_padding_color: "#49b1f5"
  blockquote_background_color: "#49b1f5"
  scrollbar_color: "#49b1f5"
  meta_theme_color_light: "rgba(255, 255, 255, .5)" # #fff
  meta_theme_color_dark: "rgba(13, 13, 13, .5)" # #0d0d0d
```


在`themes\butterfly\source\css\index.styl`中生成的样式如下：

```css
:root {
  --theme-main: #49b1f5;
  --theme-paginator: #00c4b6;
  --theme-button-hover: #ff7242;
  --theme-text-selection: #00c4b6;
  --theme-link-color: #99a9bf;
  --theme-meta-color: #858585;
  --theme-hr-color: #a4d8fa;
  --theme-code-foreground: #f47466;
  --theme-code-background: rgba(27,31,35,0.05);
  --theme-toc-color: #00c4b6;
  --theme-blockquote-padding-color: #49b1f5;
  --theme-blockquote-background-color: #49b1f5;
  --theme-scrollbar-color: #49b1f5;
  --theme-meta-theme-color-light: rgba(255,255,255,0.5);
  --theme-meta-theme-color-dark: rgba(13,13,13,0.5);
}
```