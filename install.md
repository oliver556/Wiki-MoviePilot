---
title: 安装指引
description: 如何安装 MoviePilot
published: true
date: 2024-06-18T04:39:39.936Z
tags: 
editor: markdown
dateCreated: 2024-06-18T04:39:39.936Z
---

# Docker

MoviePilot 在 docker 境像中同时还内置了`虚拟显示`、`浏览器仿真`、`内建重启`、`代理缓存`等特性，**推荐使用 docker 方式安装**。

### docker-cli

```shell
docker run -itd \
    --name moviepilot \
    --hostname moviepilot \
    -p 3000:3000 \
    -v /media:/media \
    -v /moviepilot/config:/config \
    -v /moviepilot/core:/moviepilot/.cache/ms-playwright \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    -e 'NGINX_PORT=3000' \
    -e 'PORT=3001'
    -e 'PUID=0' \
    -e 'PGID=0' \
    -e 'UMASK=000' \
    -e 'TZ=Asia/Shanghai' \
    -e 'AUTH_SITE=iyuu' \
    -e 'IYUU_SIGN=xxxx' \
    -e 'SUPERUSER=admin' \
    -e 'API_TOKEN=moviepilot' \
    --restart always \
    jxxghp/moviepilot:latest
```

### docker-compose

```shell
version: '3.3'

services:

    moviepilot:
        stdin_open: true
        tty: true
        container_name: moviepilot
        hostname: moviepilot
        networks:
            - moviepilot
        ports:
            - target: 3000
              published: 3000
              protocol: tcp
        volumes:
            - '/media:/media'
            - '/moviepilot/config:/config'
            - '/moviepilot/core:/moviepilot/.cache/ms-playwright'
            - '/var/run/docker.sock:/var/run/docker.sock:ro'
        environment:
            - 'NGINX_PORT=3000'
            - 'PORT=3001'
            - 'PUID=0'
            - 'PGID=0'
            - 'UMASK=000'
            - 'TZ=Asia/Shanghai'
            - 'AUTH_SITE=iyuu'
            - 'IYUU_SIGN=xxxx'
            - 'SUPERUSER=admin'
            - 'API_TOKEN=moviepilot'
        restart: always
        image: jxxghp/moviepilot:latest

networks:
  moviepilot:
    name: moviepilot
```

**相关说明：**
- `/media` 为媒体文件目录，根据实际情况调整，需要注意的是，**如果你计划使用`硬链接`来整理文件，那么`文件下载目录`和整理后的`媒体库目录`只能映射一个根目录不能分开映射，否则将会导致跨盘无法硬链接。** 这是由 docker 的目录映射机制决定的，下面这些情况都会导致跨盘无法硬链接：
   1. 下载目录和媒体库目录分别属于两个不同的磁盘
   2. 下载目录和媒体库目录属于同一磁盘，但在两个不同的分区/存储空间/存储池中
   3. 下载目录和媒体库目录分别作为两个目录路径映射到docker容器中
- `/moviepilot/config` 为配置文件、数据库文件、日志文件、缓存文件使用的文件目录，该目录将会存储所有设置和数据，需根据实际情况调整。
- `/moviepilot/core` 为浏览器内核下载保存目录（**避免容器重置后重新下载浏览器内核**），需根据实际情况调整。
- `/var/run/docker.sock` 用于内建重启时使用，建议映射。
- 默认使用 `3000` 为 WEB 服务端口，`3001` 为 Api 服务端口，可根据实际情况调整。
- `AUTH_SITE`、`SUPERUSER`、`API_TOKEN`等其它变量请根据 [配置参考](/configuration) 说明调整和补充，上述为最基础配置，实际可以根据需要补充其它变量。

# Windows

Windows 环境下提供两种安装方式，推荐使用`安装版本`。

### 可执行文件版本

项目在编译时，将 Python 以及相关的代码打包到一个 exe 文件中使用，点击 [此处](https://github.com/jxxghp/MoviePilot/releases) 下载 exe 文件，双击运行后会自动生成 `config` 配置文件目录，以及 `nginx` 前端资源目录，正常运行后会操作系统任务栏会生成 MoviePilot 图标，输入 `localhost:3000` 可访问后端管理 WEB。
- 如运行后无法正常解压，请使用**管理员权限**运行。
- 如被杀毒软拦截，则需要将运行目录加入**白名单**放行，因打包方式的原因有的杀毒软件会识判为病毒。
- **还需根据  [配置参考](/configuration) ，在 `Windows 系统属性 -> 高级设置 -> 环境变量`中添加认证等环境变量。**
- 由于该版本是预编译后再打包的方式，在功能上存在一定的限制，比如不支持`内建重启`、除`内置插件库`外不支持安装其它第三方插件库插件等。

### 安装版本

由 [Windows-MoviePilot](https://github.com/developer-wlj/Windows-MoviePilot) 项目提供，参考项目说明使用。

# Synology套件

DSM7 添加套件源：https://spk7.imnks.com/ ，安装后通过 `MoviePilot配置` 入口，根据  [配置参考](/configuration) 进行配置使用。

该套件由套件源作者维护，如有问题需要向套件源维护方寻求帮助。

# 源代码运行

MoviePilot 项目已拆分为多个项目，使用源码运行时需要手动将相关项目文件进行整合：
1. 使用 `git clone` 或者下载源代码包的方式下载主项目 [MoviePilot](https://github.com/jxxghp/MoviePilot) 文件到本地。
```shell
git clone https://github.com/jxxghp/MoviePilot
```
2. 将工程 [MoviePilot-Plugins](https://github.com/jxxghp/MoviePilot-Plugins) `plugins` 目录下的所有文件复制到 `app/plugins` 目录，`icons` 目录下的所有文件复制到前端项目的 `public/plugin_icon` 目录下
3. 将工程 [MoviePilot-Resources](https://github.com/jxxghp/MoviePilot-Resources) resources 目录下的所有文件复制到 `app/helper` 目录
4. 执行命令：`pip install -r requirements.txt` 安装依赖
5. 执行命令：`PYTHONPATH=. python app/main.py` 启动主服务
6. 据前端项目 [MoviePilot-Frontend](https://github.com/jxxghp/MoviePilot-Frontend) 说明，启动前端服务

# 反向代理

如需开启域名访问 MoviePilot，则需要搭建反向代理服务。以 `nginx` 为例，需要添加以下配置项，否则可能会导致部分功能无法访问（`ip:port` 修改为实际值）：

```nginx
location / {
    proxy_pass http://ip:port;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

反向代理使用 SSL 时，还需要开启 http2，否则会导致日志加载时间过长或不可用：

```nginx
server {
    listen 443 ssl;
    http2 on;
    # ...
}
```

**代理服务连接超时时间应尽量长⼀些，比如 10 分钟，避免代理服务器强制中断请求。**