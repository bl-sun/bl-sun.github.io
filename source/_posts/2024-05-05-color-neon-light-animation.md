---
title: CSS霓虹字体特效
date: 2024-05-05 20:21:22
tags:
    - CSS
    - Butterfly
    - Hexo
    - 建站
categories:
    - 建站
description:
cover: https://cbu01.alicdn.com/img/ibank/2018/925/631/9475136529_644721273.jpg
---
# CSS霓虹字体特效

今天又是抄大佬博客的一天，看了[⭐️齐下无贰⭐️](//weidows.github.io/)的博客，把大佬里面文字的霓虹字体特效扒下来了。

```css
/* 霓虹特效 */
@keyframes color-neon-light {
    0% {
        color:#0ff;
        text-shadow: 0 0 5px #fc199a, 0 0 10px rgba(252,25,154,.486);
        filter: hue-rotate(0);
    }
    100% {
        color:#0ff;
        text-shadow: 0 0 5px #fc199a, 0 0 10px rgba(252,25,154,.486);
        filter: hue-rotate(360deg);
    }
}
/* 设置静态变量用于该特效 */
:root{
    --static-kf-color-neon-light: color-neon-light 2.5s linear infinite;
}
/* 通过变量控制特效只在夜间生效 */
[data-theme='light']{
    --title-animation: none;
}
[data-theme='dark']{
    --title-animation: var(--static-kf-color-neon-light);
}
/* 在标题上设置特效 */
#site-title, #site-subtitle, .post-title{
    animation: var(--title-animation);
}
```

> 本文设置的特效只会在夜间模式生效。
> 受本人学识所限，这个霓虹灯特效的生效范围配置，多少有点不合理。欢迎您提出宝贵的[建议](https://github.com/bl-sun/bl-sun.github.io/issues)
