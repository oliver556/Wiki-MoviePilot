---
title: 基础
description: 初始密码、用户认证等基础内容
published: true
date: 2024-06-18T04:49:55.131Z
tags: 
editor: markdown
dateCreated: 2024-06-18T04:49:55.131Z
---

# 用户初始密码

为了确保安全，系统安装后第一次启动时将生成随机复杂密码，**需要从第一次启动的后台日志文件或终端日志中获取管理员用户初始密码才能登录系统**，登录后可在 `设定 → 用户` 中修改。

- 日志文件路径为 `/config` 对应映射目录下 `logs/moviepilot.log` 文件。
- 可以通过搜索关键字 `超级管理员` 快速定位密码位置。

![password.gif](/password.gif)

> 管理员用户不要使用弱密码！如非必要不要暴露到公网。如被盗取管理账号权限，将会导致站点 Cookie 等敏感数据泄露！
{.is-danger}

