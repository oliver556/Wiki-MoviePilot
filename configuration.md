---
title: 配置参考
description: 所有支持的配置项说明
published: true
date: 2024-06-17T16:41:45.841Z
tags: 
editor: markdown
dateCreated: 2024-06-17T16:17:57.352Z
---

> 除本页面说明的配置变量外，其余配置项均可以通过 MoviePiolt 的后台管理页面进行配置。
{.is-success}

> 所有配置项均支持 `环境变量`、`env配置文件`、`WEB界面`  三种配置方式，且优先级为：`环境变量` > `env配置文件` == `WEB界面`。
{.is-warning}

> ❗号标识的为必填项，其它为可选项，可选项可删除配置变量从而使用默认值。
{.is-info}

# 环境变量
- **❗NGINX_PORT：** WEB 服务端口，默认 `3000`，可自行修改，不能与 API 服务端口冲突
- **❗PORT：** API 服务端口，默认 `3001`，可自行修改，不能与 WEB 服务端口冲突
- **PUID**：运行程序用户的 `uid`，默认 `0`
- **PGID**：运行程序用户的 `gid`，默认 `0`
- **UMASK**：掩码权限，默认 `000`，可以考虑设置为 `022`
- **PROXY_HOST：** 网络代理，访问 themovied b或者重启更新需要使用代理访问，格式为 `http(s)://ip:port`、`socks5://user:pass@host:port`
- **MOVIEPILOT_AUTO_UPDATE：** 重启时自动更新，`true` / `release` / `dev` / `false`，默认`release`，需要能正常连接 Github **注意：如果出现网络问题可以配置 `PROXY_HOST`**
- **❗AUTH_SITE：** **认证站点**（认证通过后才能使用站点相关功能），支持配置多个认证站点，使用`,`分隔，如：`iyuu,hhclub`，会依次执行认证操作，直到有一个站点认证成功。  

> 配置 `AUTH_SITE` 后，需要根据下表配置对应站点的认证参数。`AUTH_SITE` 认证站点支持：`iyuu` / `hhclub`/`audiences` / `hddolby` / `zmpt` / `freefarm` / `hdfans` / `wintersakura` / `leaves` / `ptba` / `icc2022` / `xingtan` / `ptvicomo` / `agsvpt` / `hdkyl` / `qingwa` / `discfan`

**各认证站点对应参数配置如下：**

|      站点     |                          参数                            |
|:------------:|:--------------------------------------------------------|
|     iyuu     |                 `IYUU_SIGN`：IYUU登录令牌                  |
|    hhclub    |     `HHCLUB_USERNAME`：用户名<br/>`HHCLUB_PASSKEY`：密钥    |
|  audiences   |    `AUDIENCES_UID`：用户ID<br/>`AUDIENCES_PASSKEY`：密钥    |
|   hddolby    |      `HDDOLBY_ID`：用户ID<br/>`HDDOLBY_PASSKEY`：密钥       |
|     zmpt     |         `ZMPT_UID`：用户ID<br/>`ZMPT_PASSKEY`：密钥         |
|   freefarm   |     `FREEFARM_UID`：用户ID<br/>`FREEFARM_PASSKEY`：密钥     |
|    hdfans    |       `HDFANS_UID`：用户ID<br/>`HDFANS_PASSKEY`：密钥       |
| wintersakura | `WINTERSAKURA_UID`：用户ID<br/>`WINTERSAKURA_PASSKEY`：密钥 |
|    leaves    |       `LEAVES_UID`：用户ID<br/>`LEAVES_PASSKEY`：密钥       |
|     ptba     |         `PTBA_UID`：用户ID<br/>`PTBA_PASSKEY`：密钥         |
|   icc2022    |      `ICC2022_UID`：用户ID<br/>`ICC2022_PASSKEY`：密钥      |
|   xingtan    |      `XINGTAN_UID`：用户ID<br/>`XINGTAN_PASSKEY`：密钥      |
|   ptvicomo   |     `PTVICOMO_UID`：用户ID<br/>`PTVICOMO_PASSKEY`：密钥     |
|    agsvpt    |       `AGSVPT_UID`：用户ID<br/>`AGSVPT_PASSKEY`：密钥       |
|    hdkyl     |        `HDKYL_UID`：用户ID<br/>`HDKYL_PASSKEY`：密钥        |
|   qingwa     |      `QINGWA_UID`：用户ID<br/>`QINGWA_PASSKEY`：密钥        |
|   discfan    |      `DISCFAN_UID`：用户ID<br/>`DISCFAN_PASSKEY`：密钥      |

# 环境变量 / 配置文件

配置文件名：`app.env`，放配置文件根目录，点击 [此处](https://raw.githubusercontent.com/jxxghp/MoviePilot/main/config/app.env) 可下载模板。

- **❗SUPERUSER：** 超级管理员用户名，默认 `admin`，安装后使用该用户登录后台管理界面，**注意：启动一次后再次修改该值不会生效，除非删除数据库文件！**
- **❗API_TOKEN：** API 密钥，默认 `moviepilot`，在媒体服务器 Webhook、微信回调等地址配置中需要加上 `?token=` 该值，建议修改为复杂字符串
- **BIG_MEMORY_MODE：** 大内存模式，默认为 `false`，开启后会增加缓存数量，占用更多的内存，但响应速度会更快
- **DOH_ENABLE：** DNS over HTTPS 开关，`true` / `false`，默认 `true`，开启后会使用 DOH 对 api.themoviedb.org 等域名进行解析，以减少被 DNS 污染的情况，提升网络连通性
- **META_CACHE_EXPIRE：** 元数据识别缓存过期时间（小时），数字型，不配置或者配置为 0 时使用系统默认（大内存模式为 7 天，否则为 3 天），调大该值可减少 themoviedb 的访问次数
- **GITHUB_TOKEN：** Github token，提高自动更新、插件安装等请求 Github Api 的限流阈值，格式：ghp_****
- **GITHUB_PROXY：** Github 代理地址，用于加速版本及插件升级安装，格式：`https://mirror.ghproxy.com/`
- **DEV:** 开发者模式，`true` / `false`，默认 `false`，仅用于本地开发使用，开启后会暂停所有定时任务，且插件代码文件的修改无需重启会自动重载生效
- **AUTO_UPDATE_RESOURCE**：启动时自动检测和更新资源包（站点索引及认证等），`true` / `false`，默认 `true`，需要能正常连接 Github，仅支持 Docker 镜像
---

