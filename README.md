# Unraid Docker 镜像模板

## 如何使用

把此地址：https://github.com/NasUtils/unraid-docker-templates

添加到Unraid - Docker - (Template repositories)模板存储库 - SAVE(保存)

然后创建容器时就能从模板下拉菜单里找到。

镜像的下载地址都已经替换成了国内源。

-----------

## 文件管理器类

### krusader

请直接到应用市场搜索：binhex krusader，支持中文文件名。

### FileBrowser

[docker镜像发布页](https://hub.docker.com/r/80x86/filebrowser)

使用荒野无灯的docker镜像，界面支持中文，默认的管理路径是Unraid的/mnt目录。

使用模板直接创建即可，默认访问端口是8082，管理账户：admin / admin。

### kodexplorer

[docker镜像发布页](https://hub.docker.com/r/kodcloud/kodexplorer)

可道云资源管理器，使用kodcloud官方发布的kodexplorer镜像。

主机路径默认为/mnt，映射到docker内路径为/mnt/UNRAID，请使用可道云资源管理器打开/mnt/UNRAID进行管理。

使用模板直接创建即可，默认访问端口是9988，第一次访问时设置admin密码。

-----------

## 笔记类

### 为知笔记服务端wizserver

[docker镜像发布页](https://hub.docker.com/r/wiznote/wizserver)

[为知官方部署说明](https://www.wiz.cn/zh-cn/docker)

为知笔记服务端docker镜像，使用为知笔记官方发布的镜像。

数据路径：/mnt/user/appdata/wizserver.

使用模板直接创建即可，默认访问端口是9270，管理账户：admin@wiz.cn / 123456。

使用客户端时，填写的地址是：IP:9270或者域名:9270。

-----------

## 下载工具

### 百度网盘客户端baidunetdisk

[docker镜像发布页](https://hub.docker.com/r/johnshine/baidunetdisk-crossover-vnc)

百度网盘客户端。

使用前先创建一个共享download，对应的主机路径是/mnt/user/download。

首次登陆后，需要进设置，把下载路径设置为/home/baidu/baidunetdiskdownload

### transmission

先在共享里配置好下载目录，例如donwload。

请直接到应用市场搜索：transmission，找到linuxserver's Repository。

添加容器时，配置容器downloads路径映射为主机路径：/mnt/user/donwload/

配置容器watch路径映射为主机路径：/mnt/user/donwload/watch/

#### 汉化方法

容器运行后，进入Console，运行命令

```shell
wget https://gitee.com/culturist/transmission-web-control/raw/master/release/install-tr-control-gitee.sh

chmod a+x install-tr-control-cn.sh

./install-tr-control-cn.sh
```

### aria2pro

更好的Aria2容器镜像

[docker镜像发布页](https://hub.docker.com/r/p3terx/aria2-pro)

[github项目地址](https://github.com/P3TERX/Aria2-Pro-Docker)

[帮助文档](https://p3terx.com/archives/docker-aria2-pro.html)

安装容器前，先配置共享文件夹download，安装容器时，要配置下载路径/downloads，映射到主机路径/mnt/user/download。

-----------

## 内网穿透

### frps服务端

[docker镜像发布页](https://hub.docker.com/r/snowdreamtech/frps)

[github项目地址](https://github.com/fatedier/frp)

[帮助文档](https://github.com/fatedier/frp/blob/dev/README_zh.md)

frp 是一个专注于内网穿透的高性能的反向代理应用，支持 TCP、UDP、HTTP、HTTPS 等多种协议。可以将内网服务以安全、便捷的方式通过具有公网 IP 节点的中转暴露到公网。

frps是服务端，frpc是客户端。

容器内的配置文件路径是/etc/frp，映射到unraid主机/mnt/user/appdata/frps路径

创建后第一次启动会失败，因为没有配置文件。

此时，需要从[下载](https://github.com/fatedier/frp/releases)，随便找一个下载解压，找到frps.ini配置文件。

把frps.ini文件放入/mnt/user/appdata/frps路径，再重启容器。

### frpc客户端

一般装到window、linux、macOS主机上，安装方法与frps类似。

容器内的配置文件路径是/etc/frp，映射到unraid主机/mnt/user/appdata/frpc路径

创建后第一次启动会失败，因为没有配置文件。

此时，需要从[下载](https://github.com/fatedier/frp/releases)，随便找一个下载解压，找到frpc.ini配置文件。

把frpc.ini文件放入/mnt/user/appdata/frpc路径，再重启容器。

### nps服务端

[docker镜像发布页](https://hub.docker.com/r/ffdfgdfg/nps)

[github项目地址](https://github.com/ehang-io/nps)

[帮助文档](https://ehang-io.github.io/nps)

一款轻量级、高性能、功能强大的内网穿透代理服务器。支持tcp、udp、socks5、http等几乎所有流量转发，

可用来访问内网网站、本地支付接口调试、ssh访问、远程桌面，内网dns解析、内网socks5代理等等……，并带有功能强大的web管理端。

默认端口配置，可以在创建时自己更改:

| 容器端口     | 主机端口  | 作用 |
| ------- | ------- | ----: |
| 80  | 18000 | 域名解析模式端口 |
| 443  | 18443 | 域名解析模式端口 |
| 8080 | 18080  | web管理访问端口 |
| 8024 | 18024 | 客户端与服务器通信 |

容器内的配置文件路径是/conf，映射到unraid主机/mnt/user/appdata/nps路径

创建后第一次启动会失败，因为没有配置文件。

此时，需要把从项目页面下载[conf文件夹](https://github.com/ehang-io/nps/tree/master/conf)

把文件放入/mnt/user/appdata/nps路径

最终目录接口为：

```

/mnt/user/appdata/nps ├── clients.json ├── hosts.json ├── multi_account.conf ├── npc.conf ├── nps.conf ├── server.key
├── server.pem └── tasks.json

```

默认管理账户：admin / 123
