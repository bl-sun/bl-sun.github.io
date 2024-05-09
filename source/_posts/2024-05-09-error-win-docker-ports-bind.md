---
title: 启动docker compose mysql提示端口号被占用的错误
date: 2024-05-09 16:07:57
updated: 2024-05-09 16:07:57
tags:
  - 经验
categories:
  - [经验]
description:
cover: https://getzhuji.com/wp-content/uploads/2019/11/docker.webp
---

# 启动docker compose mysql提示端口号被占用的错误

```
Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:3306 -> 0.0.0.0:0: listen tcp 0.0.0.0:3306: bind: Only one usage of each socket address (protocol/network address/port) is normally permitted.
```

查看使用3306端口号：

```powershell
netstat -aon|findstr 3306
```
```
  TCP    192.168.10.73:3306     104.17.143.163:443     ESTABLISHED     46132
```

查看指定 PID 的进程
```powershell
tasklist|findstr 46132
```
```
Cloudflare WARP.exe          46132 Console                   14    347,388 K

```

结束进程
```powershell
taskkill /T /F /PID 46132
```


通过cmd命令查看哪些端口被禁用TCP协议：

```powershell
netsh interface ipv4 show excludedportrange protocol=tcp
```
```
协议 tcp 端口排除范围

开始端口    结束端口
----------    --------
      5357        5357
      8884        8884
     50000       50059     *

* - 管理的端口排除。
```

停止NAT网络：

```powershell
net stop winnat
```

启动NAT网络：

```powershell
net start winnat
```

## 参考

- [win10使用Docker Desktop启动mysql报错](https://www.cnblogs.com/sowler/p/17567703.html)
- [windows查看占用端口](https://www.cnblogs.com/sowler/p/17164166.html)
