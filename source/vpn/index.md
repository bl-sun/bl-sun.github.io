---
title: IMUT白嫖校园网
date: 2024-06-05 21:14:24
description:
top_img:
---

## 起因

其实是偶然间发现连接校园网WiFi可以ping到实验室的机器，因此萌生了白嫖校园网的想法。

简而言之就是通过实验室的主机上网

## 连接

1. [下载](https://cdn.jsdelivr.net/gh/bl-sun/bl-sun.github.io@main/source/vpn/ca.cer)并导入证书
    1. winodws一般导入到**受信任的根证书颁发机构**
    2. 手机等设备导入请自行百度
1. 首先连接名为IMUT-Student的校园网WiFi
2. 连接vpn，如图：![](https://cdn.jsdelivr.net/gh/bl-sun/bl-sun.github.io@main/source/vpn/vpn-config.jpg)
3. 如果没有网络需要**登录账号**：连接完成后在浏览器输入[2.2.2.2](2.2.2.2)进入学校认证页面，然后登录自己的账号即可。相当于在实验室上网

## 原理

## 部署

- 实验室主机
    - ip: 10.131.66.111
    - username: sss
    - password: sss

## 参考

- [one-key-ikev2.sh](https://github.com/quericy/one-key-ikev2-vpn/blob/master/one-key-ikev2.sh#L499)

