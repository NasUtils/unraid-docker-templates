# Unraid Docker 镜像模板

## 如何使用

把此地址：https://github.com/NasUtils/unraid-docker-templates

添加到Unraid - Docker - (Template repositories)模板存储库 - SAVE(保存)

然后创建容器时就能从模板下拉菜单里找到。

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

### 为知笔记Nginx Proxy Manager配置

除了常规的代理配置选项以外，还必须在Adviced - Custom Nginx Configuration里添加
```
location / {
    proxy_pass http://IP:9270;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header x−wiz−real−ip $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

-----------
### WebDav

添加容器，配置如下
```
名称:随便
存储库:ugeek/webdav:amd64
WebUI:http://[IP]:[PORT:9090]
网络类型:Bridge
添加路径: 名称:share,容器路径:/media,主机路径:自己配置
添加变量: 名称:USERNAME,键:USERNAME,值:自己配置
添加变量: 名称:PASSWORD,键:PASSWORD,值:自己配置
添加变量: 名称:PUID,键:PUID,值:99
添加变量: 名称:PGID,键:PGID,值:100
添加端口: 名称:port,容器端口:80,主机端口:9090,连接类型:TCP
```

-----------

### joplin笔记服务端(不建议使用)

### *注意事项: joplin客户端频繁，而服务端更新没跟上，容易造成同步问题，故不建议使用，建议直接使用webdav作为服务端*

#### 先安装PostgresSQL

joplin server依赖数据库，如果不配置数据库，默认会使用SQLite(用于测试)。

应用市场搜索：Postgres

找到Postgres12.5，作者是Flight777。

配置选项：

因为数据库就在内网使用(不提供外网访问)，且单独提供给joplin，所以就按如下简单配置即可。

POSTGRES_PASSWORD: joplin ，访问数据库的密码

POSTGRES_USER: joplin，访问数据库的用户名

POSTGRES_DB: joplin，数据库名称

Database Storage Path: /mnt/user/appdata/joplindb/, 数据文件存储位置，根据你实际情况配置

Web Interface Port: 5432，数据库端口，默认即可，如果有冲突或由多个数据库实例，也可以自行更改。

#### 安装joplin server

请直接到应用市场搜索：joplin，作者是Muwahhidun。

配置选项：

假设unRaid的IP地址是:192.168.1.2

Container Port: 22300，joplin服务端口号，可以自由配置

APP_BASE_URL: 建议先配置为内网访问，例如：http://192.168.1.2:22300，端口号根据上一步来配置

后续配置好后，再更改为外网访问配置，例如：http(s)://joplin.domain.com:22300，也可以加nginx代理来实现https。

DB_CLIENT:pg，数据库类型，不需要更改，pg代表Postgres

POSTGRES_DATABASE:joplin，数据库名称，Postgres安装时配置的名称

POSTGRES_USER:joplin，数据库user，Postgres安装时配置的用户名

POSTGRES_PASSWORD:joplin，数据库密码，Postgres安装时配置的密码

POSTGRES_HOST：192.168.1.2(例)，数据库的IP地址，Postgres安装于哪台设备它的IP，这里填写unraid的IP地址即可

POSTGRES_PORT：数据库端口，Postgres安装时配置的端口

#### 初次运行配置

浏览器打开http://192.168.1.2:22300，提示跳到http://192.168.1.2:22300/login登陆。

默认管理员账号：admin@localhost / admin

添加用户账号：Users - Add user - 填写好信息 - Create user

#### 配置外网访问

申请(购买)SSL证书，或使用生成自签名证书。

nginx可以使用Nginx Proxy Manager，配置SSL，添加Proxy Hosts。

unRaid - DOCKER - joplin - 编辑 - APP_BASE_URL:改为外网ip+端口。

## 下载工具

### 百度网盘客户端baidunetdisk

[docker镜像发布页](https://hub.docker.com/r/johnshine/baidunetdisk-crossover-vnc)

百度网盘客户端。

使用前先创建一个共享download，对应的主机路径是/mnt/user/download。

首次登陆后，需要进设置，把下载路径设置为/home/baidu/baidunetdiskdownload

### qBittorrent(推荐，自带多国语言web界面)
1. 应用中心搜索qBittorrent，找到linuxserver/qbittorrent
2. 配置/downloads路径，其他默认，默认的web登陆端口是8080，首次登陆必需先用默认值。
3. 登录UI，admin/adminadmin
4. 修改设置页WebUI语言为中文
5. 如果要改web端口，先取消勾选"启用 Host header 属性验证"，停止容器，编辑docker属性，把端口(Host Port 3)从8080改为例如6882
6. 如果不想每次局域网访问都登陆，勾选"对IP子网白名单中的客户端跳过身份验证" 填入自己局域网IP网段，如：192.168.1.0/24 

### transmission(因为版本更新很快，汉化没有更新，建议使用qBittorrent)

先在共享里配置好下载目录，例如donwload。

请直接到应用市场搜索：transmission，找到linuxserver's Repository。

添加容器时，配置容器downloads路径映射为主机路径：/mnt/user/donwload/

配置容器watch路径映射为主机路径：/mnt/user/donwload/watch/

#### 汉化方法(汉化项目更新已经跟不上了，不建议使用)

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

添加模板存储库地址：https://github.com/P3TERX/unraid-docker-templates

安装容器前，先配置共享文件夹download，安装容器时，要配置下载路径/downloads，映射到主机路径/mnt/user/download。

默认RPC_SECRET=P3TERX

### ariaNG

[docker镜像发布页](https://hub.docker.com/r/p3terx/ariang)

[github项目地址](https://github.com/mayswind/AriaNg)

[帮助文档](https://p3terx.com/archives/aria2-frontend-ariang-tutorial.html)

启动后，先进入AriaNg设置-RPC页签-Aria2RPC密钥：P3TERX

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

### nps服务端(个人更推荐frp)

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
/mnt/user/appdata/nps
├── clients.json
├── hosts.json
├── multi_account.conf
├── npc.conf
├── nps.conf
├── server.key
├── server.pem
└── tasks.json
```

默认管理账户：admin / 123
