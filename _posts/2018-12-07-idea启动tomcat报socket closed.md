---
layout: post
title: idea启动tomcat socket closed
keywords: 
category: java
author: 晴天
description:
---

原因是端口被占用。

解决办法(win)：

```
查看指定端口的连接信息
netstat -ano | findstr "8080"

查看进程列表，查找进程名包含"java"的所有进程
tasklist | findstr "java"

通过以上方式可以查看占用端口号的进程
taskkill -PID 进程号 -F
```

