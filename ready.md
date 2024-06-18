---
title: 环境准备
description: 安装前的准备工作
published: true
date: 2024-06-18T04:32:47.091Z
tags: 
editor: markdown
dateCreated: 2024-06-17T05:56:00.011Z
---

# 网络
MoviePilot 通过调用 [TheMovieDb](https://api.themoviedb.org) 的 Api 来读取和匹配媒体元数据，通过访问 [Github](https://github.com) 来执行程序升级、安装插件等。有顺畅连接上述网址的网络环境是能流畅使用本软件的前提。**推荐使用前两种方式**，网络质量更加稳定。

### 单独代理
搭建代理服务，并将代理地址填入 MoviePilot 的环境变量中，软件会自动对需要使用代理的请求使用代理服务器。具体可参考 [配置参考](/configuration) 章节设置代理服务器变量。

### 全局代理
将 MoviePilot 所在的网络接入代理，通过分流规则将软件的网络请求通过代理发出，同时剔除站点相关的网络请求。

### 防域名污染与中转加速
- 更换 TheMovieDb 的 Api 地址为 `api.tmdb.org`、开启`DOH`、本地修改 `hosts` 文件协持 `api.themoviedb.org` 域名地址为可访问 IP、使用 `Cloudflare Workers` 搭建代理中转等，综合使用以上方式调优 TheMovieDb 的网络访问，涉及调整系统设定的参考 [配置参考](/configuration) 章节。
- 使用 Github 中转加速服务器来加快 Github 文件下载请求，具体可参考 [配置参考](/configuration) 章节。

#### Cloudflare Workers 参考代码：
```javascript
async function handleRequest(request) {
  // 从请求URL中获取 API查询参数
  const url = new URL(request.url)
  const searchParams = url.searchParams

  // 设置代理API请求的URL地址
  const apiUrl = `https://api.themoviedb.org/${url.pathname}?${searchParams.toString()}`

  // 设置API请求的headers
  const headers = new Headers(request.headers)
  headers.set('Host', 'api.themoviedb.org')
  
  // 创建API请求
  const response = await fetch(apiUrl, {
    method: request.method,
    headers: headers
  })

  // 返回API响应
  return response
}

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})
```

# 操作系统
部分功能基于文件系统监控实现（如`目录监控`等），监控的文件较多时，往往会因为操作系统默认允许的文件句柄数太小导致报错，相关功能失效，需在**宿主机**操作系统上（不是docker容器内）执行以下命令并重启生效：

```shell
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf
echo fs.inotify.max_user_instances=524288 | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

# 站点
MoviePilot 包括两大部分功能：`文件整理刮削`、`资源订阅下载`，其中`资源订阅下载`功能需要有可用的 `PT站点`，**同时这些站点中需要有一个可用于认证**，关于用户认证请参考 [基础](/basic) 章节的相关说明。

# 配套软件
MoviePilot 只是媒体库自动化管理的一环，需要通过调用`下载器`来完成资源的下载，需要通过`媒体服务器`来管理和展示媒体资源，**同时通过媒体服务器 Api 来查询库存情况控制重复下载**，通过`CookieCloud`来快速同步站点 Cookie 和新增站点。安装前需要先完成配套软件的安装。

### 下载器
- **Qbittorrent**：版本要求 >= `4.3.9`
- **Transmission**：版本要求 >= `3.0`

### 媒体服务器
- **Emby**：建议版本 >= `4.8.0.45`
- **Jellyfin**：推荐使用 `latest` 分支
- **Plex**：无特定版本要求

### CookieCloud
- **CookieCloud 服务端**：可选，MoviePilot 已经内置了 CookieCloud 服务端，如需独立安装可参考 [easychen/CookieCloud](https://github.com/easychen/CookieCloud) 说明
- **CookieCloud 浏览器插件**：不管是使用 CookieCloud 独立服务端还是使用内置服务，都需要安装浏览器插件，访问 [此处](https://github.com/easychen/CookieCloud/releases) 下载安装到浏览器。

### Docker 管理器
如果你计划使用 docker 来部署 MoviePilot，请确认你的环境是否可以方便地编辑容器配置，否则建议安装 [portainer](https://github.com/portainer/portainer) 来简化容器操作。
```shell
docker run -d --restart=always --name="portainer" -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock 6053537/portainer-ce
```

### Overseerr/Jellyseerr
如果你希望将 MoviePilot 的自动化媒体管理能力开放给多个人使用，同时具有用户提交订阅申请与集中审批的功能，可以安装 `Overseerr` / `Jellyseerr`来配合实现更好的选片和申请审批使用体验。MoviePilot 通过模拟 `Radarr` 和 `Sonarr` 的 Api 实现无缝集成，`Overseerr` / `Jellyseerr`负责选片和用户权限管理，MoviePilot 负责订阅、下载和整理。参考下图：

