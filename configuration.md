---
title: 配置参考
description: 所有支持的配置项说明
published: true
date: 2024-06-17T16:21:51.345Z
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

